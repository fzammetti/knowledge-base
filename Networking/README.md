# Networking

---

* [ExpressVPN Adapter Binding In QBitorrent](#89691b66-293b-4383-a745-c7ddd69e1ca3)

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
