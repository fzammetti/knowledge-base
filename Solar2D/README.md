# Solar2D

---

* [To ensure images to work on Android, use this checklist](#2514197c-4fe9-4563-a513-43ef1025c562)
* [General things you CANNOT do](#d42dfbda-91a7-4c79-b08b-e7ee54c93a23)
* [General things you CAN do](#1ac906cc-e568-4029-80ac-3a7d01e64bd8)
* [Debug Info Code](#e990dd52-2556-437d-a724-a4b558a36eb7)
* [Letterbox Scaling Info](#a01cfe21-5385-45b2-b832-e49532149d53)

---




<div id="2514197c-4fe9-4563-a513-43ef1025c562">

## To ensure images to work on Android, use this checklist

</div>

* Image has to be RGB color 8-bit
* Image cannot have ICC embedded profile
* Image cannot have reserved Java names
* Image cannot have _ # or weird characters
* If two or more files have the same name but different extensions that won't work
* Correct uppercase/lowercase devices enforce that casing must match




<div id="d42dfbda-91a7-4c79-b08b-e7ee54c93a23">

## General things you CANNOT do

</div>

* Use the file system to access files in subdirectories (off the Resource directory)
* Use the file system to access image or html files in Resource directory
* Have sound files in subdirectories
* Have sound files with the same name but different extensions in the Resource directory




<div id="1ac906cc-e568-4029-80ac-3a7d01e64bd8">

## General things you CAN do

</div>

* Display images from the Resource directory
* Display images from subdirectories (off the Resource directory)
* Display images from the Documents and Temporary directories
* Play sound files from Resource, Documents, and Temporary directories (using openAL API)
* Use the file system to access: txt, xml, sound, ppp and db files in the Resource directory (I don't have a complete list but for now only image and html files seem restricted)
* Copy the above files from the Resource directory to the Documents or Temporary directory
* Copy image and html files from the Resource directory to the Documents or Temporary directory by changing the file extension (e.g., ppp) and renaming it back to the original file name after the copy.




<div id="e990dd52-2556-437d-a724-a4b558a36eb7">

## Debug Info Code

</div>

    -- Debug info variables.
    underlay = nil,
    displayInfo = nil,
    prevTime = 0,
    curTime = 0,
    dt = 0,
    fps = 60,
    mem = 0,
    frameCount = 0,
    avg = 0,
    slowest = 1000

    -- Function that updates the debug info text when active.
    function utils:updateDebugInfoText()

      utils.curTime = system.getTimer();
      utils.dt = utils.curTime - utils.prevTime;
      utils.prevTime = utils.curTime;
      utils.fps = math.floor(1000 / utils.dt);
      utils.mem = system.getInfo("textureMemoryUsed") / 1000000;
      if utils.fps > 60 then
        utils.fps = 60
      end
      utils.frameCount = utils.frameCount + 1;
      if utils.frameCount > 150 then
        utils.avg = utils.avg + utils.fps;
        if utils.fps < utils.slowest then
          utils.slowest = utils.fps;
        end
      end
      local a = math.round(utils.avg / (utils.frameCount - 150));
      a = math.floor(a * math.pow(10, 0) + 0.5) / math.pow(10, 0);
      collectgarbage()
      local sysMem = collectgarbage("count") * 0.001;
      sysMem = math.floor(sysMem * 1000) * 0.001;
      utils.displayInfo.text = "FPS: " .. utils.fps .. ", Avg: " .. a ..
        ", Min: " .. utils.slowest ..
        ", mTx: " .. string.sub(utils.mem, 1, string.len(utils.mem) - 4) .. "mb" ..
        ", mSys: " .. sysMem .. "mb" ..
        ", tCPU: " .. math.round((system.getTimer() / 1000)) .. "s";
      utils.underlay:toFront()
      utils.displayInfo:toFront()

    end -- End updateDebugInfoText().

    -- Shows debug info.
    function utils:showDebugInfo()

      utils.underlay = display.newRect(
        0, display.contentHeight - 30, display.contentWidth, 34
      );
      utils.underlay:setReferencePoint(display.TopLeftReferencePoint);
      utils.underlay:setFillColor(0, 0, 0, 128);
      utils.displayInfo = display.newText(
        "<test>", 0, 0, native.systemFontBold, 20
      );
      utils.displayInfo.x = display.contentCenterX;
      utils.displayInfo.y = display.contentHeight - 14;
      Runtime:addEventListener("enterFrame", utils.updateDebugInfoText)

    end -- End showDebugInfo().

    -- Hides debug info.
    function utils:hideDebugInfo()

      utils.underlay:removeSelf();
      utils.underlay = nil;
      utils.displayInfo:removeSelf();
      utils.displayInfo = nil;
      Runtime:removeEventListener("enterFrame", utils.updateDebugInfoText)

    end -- End hideDebugInfo().




<div id="a01cfe21-5385-45b2-b832-e49532149d53">

## Letterbox Scaling Info

</div>

Let's assume you're coding on portrait orientation.

So, you set your **config.lua** for a screen of 320x480 pixels, letterbox scalling, and position centered both vertical and horizontal:

    application = {
      content = {
        width = 320,
        height = 480,
        scale = "letterbox",
        xAlign = "center",
        yAlign = "center",
      }
    }

Now, when you create a background for the game, and you want to target iOS devices, so resolutions 320x480, 640x960 and 768x1024, you should do it like:

    local bg = display.newImageRect("bg.png",360,480)
    bg.x = display.contentWidth/2
    bg.y = display.contentHeight/2

The resolution of the image files should be:

* **bg.png** - 360x480
* **bg@2.png** - 720x960, or even 768x1024 if you want it to look really good on iPad.

Why this 360 pixel width?

Well, the thing is that with letter box, the images on higher resolutions than defined on config.lua will be scaled to fit the screen, but they won't lose their aspect ratio, and that 320x480 rectangle you define on config.lua never goes out off screen.

Let's see some examples. Start by assuming we did things the "normal" way. You call background images as 320x480, what would happen is:

* iPhone - No scalling, config.lua already defines a rectangle of the size of the screen, so it would look fine.
* iPhone4 - Scaling by a factor of 2, so the ratio is the same. Would look fine too, it would use double resolution image (640x960)
* iPad - the 320x480 rect would grow by a factor of 2.1333.., ending up at a size of 682x1024. Since the iPad has a 768x1024 screen, you would notice some black bars on the sides.

Now lets assume you did as I said and called background images as 360x480.

* **iPhone** - No scalling since config.lua defines a rectangle of screen size. The background though is a bit bigger for the screen and has some extra width compared to screen size. Since the background is centered, you would actually not see some pixels to the sides. More exactly, 360-320 = 40, so 20 pixels to each side.
* **iPhone4** - Exactly the same case but on higher resolution, scalling by a factor of 2, usage of high res images.
* **iPad** - Once again it will grow by a factor of 2.133, so a 360x480 images would grow to 768x1024, completely filling the iPad screen. No black bars. Usage of high resolution images.

Basically, using this you create the backgrounds with some extra areas that will only be shown when the screen has adifferent ratio. The screen will always be filled. You have to keep in mind though that anything out of the 320x480 center rectangle of an image (considering low res images), will not show on some devices, so don't base you game in that areas, use it just for filling.

On Android the case is a bit different since the screen is actually taller (on portrait). So instead of having to increase width of background images from 320 to 360, you have to increase their height, from 480 to...

...well, considering all available android devices that number is 570.

In the case you want to completely support all resolutions of all phones/tablets available today, you should put in **config.lua**:

    application = {
      content = {
        width = 320,
        height = 480,
        scale = "letterbox",
        xAlign = "center",
        yAlign = "center",
      }
    }

Instantiation of images for backgrounds:

    local bg = display.newImageRect("bg.png",360,570)
    bg.x = display.contentWidth/2
    bg.y = display.contentHeight/2

* **Low res images**: 360x570 pixels
* **High res images**: 720x1120 pixels

Remember that when you're coding though, you're coding for the rect defined on config.lua, so you're coding for a screen of 320x480. If for example on iPad you want to refer to the left of the screen it will not be 0, as 0 is a bit distant from the border, as iPad is wider and this 320x480 rect preserves aspect ratio. So you should actualy rely on some corona defined values to help you. For any device you can define the top, left, right and bottom points as:

* **Top**: local topY = display.screenOriginY
* **Right**: local rightX = display.contentWidth - display.screenOriginX
* **Bottom**: local bottomY = display.contentHeight - display.screenOriginY
* **Left**: local rightX = display.screenOriginX
