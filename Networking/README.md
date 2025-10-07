# Networking

---

* [ExpressVPN Adapter Binding In QBitorrent](#89691b66-293b-4383-a745-c7ddd69e1ca3)
* [Get a list of cipher suites available on a URL (Linux)](#3a292c69-c49a-4d79-9fd8-17646eb35965)
* [SFTP "Unable to negotiate" error (Linux)](#d5113dea-fed2-4c20-983b-4cff97e67477)

---




<div id="89691b66-293b-4383-a745-c7ddd69e1ca3">

## ExpressVPN Adapter Binding In QBitorrent

</div>

When trying to bind QBitorrent to the ExpressVPN adapter, remember that ExpressVPN creates TWO adapters, a TAP adapter
and a TUN adapter.  The TUN adapter is the one you need to bind.  But, when installed, ExpressVPN creates the adapter
with a name something like "Local Area Connection".  If you look in Windows Control Panel's Network Adapeter page,
you can identify which it TAP and which is TUN.  I suggest renaming both like "ExpressVPN TAP Adapter" and
"ExpressVPN TUN Adapter", then those names will appear in QBitorrent's Advanced settings, making it easy to identify
which to bind.




<div id="3a292c69-c49a-4d79-9fd8-17646eb35965">

## Get a list of cipher suites available on a URL (Linux)

</div>

    nmap --script ssl-enum-ciphers -p 443 MY_DOMAIN




<div id="d5113dea-fed2-4c20-983b-4cff97e67477">

## SFTP "Unable to negotiate" error (Linux)

</div>

Sometimes, when using a Linux SFTP client, you'll get an error saying the client was unable to negotiate with the
server, followed by a listing of algorithms that were offered.  This can happen when the server only offers an
older/weaker host-key types and the client won't allow it.  To deal with that, add these two switches to the SFTP
command line:

    -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa

You may have to change **ssh-rsa** there, but the point is to use one that the server offers.
