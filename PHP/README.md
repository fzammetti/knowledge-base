# PHP
----------

## Parse posted JSON request

    if ($_SERVER["REQUEST_METHOD"] == "POST") {
      $postBody = file_get_contents("php://input");
      $obj = json_decode($postBody);
      $field1 = $obj->{"field1"};
      echo "field1=" . field1;
    }

## Sanitize database inputs

When inserting data in your database, you have to be really careful about SQL injections and other attempts to insert malicious data into the db. The function below is probably the most complete and efficient way to sanitize a string before using it with your database in PHP:

    function cleanInput($input) {
      $search = array(
        '@<script[^>]*?>.*?</script>@si', // Strip out javascript
        '@<[\/\!]*?[^<>]*?>@si',          // Strip out HTML tags
        '@<style[^>]*?>.*?</style>@siU',  // Strip style tags properly
        '@<![\s\S]*?--[ \t\n\r]*>@'       // Strip multi-line comments
      );
        $output = preg_replace($search, '', $input);
        return $output;
      }
    ?>

    function sanitize($input) {
      if (is_array($input)) {
        foreach($input as $var=>$val) {
          $output[$var] = sanitize($val);
        }
      } else {
        if (get_magic_quotes_gpc()) {
          $input = stripslashes($input);
        }
        $input  = cleanInput($input);
        $output = mysql_real_escape_string($input);
      }
      return $output;
    }

Here are some examples of use:

    $bad_string = "Hi! <script src='http://www.evilsite.com/bad_script.js'></script> It's a good day!";
    $good_string = sanitize($bad_string);
    // $good_string returns "Hi! It\'s a good day!"

    // Also use for getting POST/GET variables
    $_POST = sanitize($_POST);
    $_GET  = sanitize($_GET);

(original source: http://css-tricks.com/snippets/php/sanitize-database-inputs)

## Calculate distance between two points

Want to be able to calculate the distance between two points on a map? The function below use the latitude and longitude of two locations, and calculate the distance between them in both miles and metric units.

    function getDistanceBetweenPointsNew($latitude1, $longitude1, $latitude2, $longitude2) {
      $theta = $longitude1 - $longitude2;
      $miles = (sin(deg2rad($latitude1)) * sin(deg2rad($latitude2))) +
        (cos(deg2rad($latitude1)) * cos(deg2rad($latitude2)) *
        cos(deg2rad($theta)));
      $miles = acos($miles);
      $miles = rad2deg($miles);
      $miles = $miles * 60 * 1.1515;
      $feet = $miles * 5280;
      $yards = $feet / 3;
      $kilometers = $miles * 1.609344;
      $meters = $kilometers * 1000;
      return compact('miles', 'feet', 'yards', 'kilometers',' meters');
    }

Example:

    $point1 = array('lat' => 40.770623, 'long' => -73.964367);
    $point2 = array('lat' => 40.758224, 'long' => -73.917404);
    $distance = getDistanceBetweenPointsNew($point1['lat'], $point1['long'], $point2['lat'], $point2['long']);
    foreach ($distance as $unit => $value) {
      echo $unit . ': ' . number_format($value, 4) . '<br />';
    }

(original source: http://www.inkplant.com/code/calculate-the-distance-between-two-points.php)

Get all tweets of a specific hashtag

## Here's a quick and easy way to get all tweets of a specific usage using the useful cURL library. The following example will retrieve all tweets with the #cat hashtag.

    function getTweets($hash_tag) {
      $url = 'http://search.twitter.com/search.atom?q='.urlencode($hash_tag);
      echo "<p>Connecting to <strong>$url</strong> ...</p>";
      $ch = curl_init($url);
      curl_setopt ($ch, CURLOPT_RETURNTRANSFER, TRUE);
      $xml = curl_exec ($ch);
      curl_close ($ch);
      //If you want to see the response from Twitter, uncomment this part out:
      //echo "<p>Response:</p>";
      //echo "<pre>".htmlspecialchars($xml)."</pre>";
      $affected = 0;
      $twelement = new SimpleXMLElement($xml);
      foreach ($twelement->entry as $entry) {
        $text = trim($entry->title);
        $author = trim($entry->author->name);
        $time = strtotime($entry->published);
        $id = $entry->id;
        echo "<p>Tweet from ".$author.": <strong>".$text."</strong>  <em>Posted ".date('n/j/y g:i a',$time)."</em></p>";
      }
      return true ;
    }

    getTweets('#cats');

(original source: http://www.inkplant.com/code/get-twitter-posts-by-hashtag.php)

## Applying Even/Odd Classes

When generating lists or tables using php, it is super useful to apply even/odd classes to each row of data in order to simplify CSS styling.

Used inside a loop, class names would be named .example-class0 and .example-class1 alternating. Increasing the "2? number allows you to increment in thirds or fourths or whatever you need:

    <div class="example-class<?php echo ($xyz++%2); ?>">

(original source: http://css-tricks.com/snippets/php/applying-evenodd-classes)

## Email error logs to yourself

Instead of publicly displaying possible errors on your website, why not using a custom error handler to email error logs to yourself? Here's a handy code snippet to do it.

    // Our custom error handler
    function nettuts_error_handler($number, $message, $file, $line, $vars) {
    	$email = "
    		<p>An error ($number) occurred on line
    		<strong>$line</strong> and in the <strong>file: $file.</strong>
    		<p> $message </p>";
    	$email .= "<pre>" . print_r($vars, 1) . "</pre>";
    	$headers = 'Content-type: text/html; charset=iso-8859-1' . "\r\n";
    	// Email the error to someone...
    	error_log($email, 1, 'you@youremail.com', $headers);
    	// Make sure that you decide how to respond to errors (on the user's side)
    	// Either echo an error message, or kill the entire project. Up to you...
    	// The code below ensures that we only "die" if the error was more than
    	// just a NOTICE.
    	if (($number !== E_NOTICE) && ($number < 2048)) {
    		die("There was an error. Please try again later.");
    	}
    }

    // We should use our custom function to handle errors.
    set_error_handler('nettuts_error_handler');

    // Trigger an error... (var doesn't exist)
    echo $somevarthatdoesnotexist;

(original source: http://net.tutsplus.com/tutorials/php/quick-tip-email-error-logs-to-yourself-with-php)

## Automatically creates variables with the same name as the key in the POST array

This snippet is very helpful for every POST processing. All you need is an array with expected keys in the POST array. This snippet automatically creates variables with the same name as the key in the POST array. If the key is not found in the POST array the variable is set to NULL. Basically you dont need to write:

    $username=$_POST["username"];
    $age=$_POST["age"];
    ...etc...

This snippet will do this boring part of every PHP code with POST handling so you can fully focus on a validation of the input, because that is much more important.

    $expected=array('username','age','city','street');
    foreach ($expected as $key) {
      if (!empty($_POST[$key])) {
        ${key}=$_POST[$key];
      } else{
        ${key}=NULL;
      }
    }

(original source: http://www.catswhocode.com/blog/snippets/automatically-creates-variables)

## Download & save a remote image on your server using PHP

Here's a super easy and efficient way to download a remote image and save it on your own server.

    $image = file_get_contents('http://www.url.com/image.jpg');
    file_put_contents('/images/image.jpg', $image); //save the image on your server

(original source: http://www.catswhocode.com/blog/snippets/download-save-a-remote-image)

## Create data URIs

Data URIs can be useful for embedding images into HTML/CSS/JS to save on HTTP requests, at the cost of maintainability. You can use online tools to create data URIs, or you can use the simple PHP function below:

    function data_uri($file, $mime) {
      $contents = file_get_contents($file);
      $base64 = base64_encode($contents);
      echo "data:$mime;base64,$base64";
    }

(original source: http://css-tricks.com/snippets/php/create-data-uris)

## Detect browser language

When developing a multilingual website, I really like to retrieve the browser language and use this language as the default language for my website. Here's how I get the language used by the client browser:

    function get_client_language($availableLanguages, $default='en') {
     if (isset($_SERVER['HTTP_ACCEPT_LANGUAGE'])) {
    		$langs = explode(',',$_SERVER['HTTP_ACCEPT_LANGUAGE']);
    		foreach ($langs as $value){
    			$choice = substr($value,0,2);
    			if (in_array($choice, $availableLanguages)) {
    				return $choice;
    			}
    		}
    	}
    	return $default;
    }

(original source: http://snipplr.com/view/12631/detect-browser-language/php-detect-browser-language)

## Add (th, st, nd, rd, th) to the end of a number

This simple and easy function will take a number and add "th, st, nd, rd, th" after it. Very useful!

    function ordinal($cdnl){
      $test_c = abs($cdnl) % 10;
      $ext = ((abs($cdnl) %100 < 21 && abs($cdnl) %100 > 4) ? 'th'
       : (($test_c < 4) ? ($test_c < 3) ? ($test_c < 2) ? ($test_c < 1)
       ? 'th' : 'st' : 'nd' : 'rd' : 'th'));
     return $cdnl.$ext;
    }
    for ($i=1; $i<100; $i++) {
      echo ordinal($i) . '<br>';
    }

(original source: http://phpsnips.com/snip-37)
