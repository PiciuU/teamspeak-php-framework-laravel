Introduction
============

Clone of [TS3 PHP Framework](https://github.com/fkubis/teamspeak-php-framework) providing fix on string issues for PHP 7.4


Usage
=====

Install via composer:

    "require": {
        "fkubis/teamspeak-php-framework": "dev-master"
    },

Ski the required_once part of official documentation and replace it with use TeamSpeak3\TeamSpeak3 statement.

Examples for API Controller:

```php
namespace App\Http\Controllers;
use TeamSpeak3\TeamSpeak3;
use Illuminate\Http\Request;

class TeamSpeak3Controller extends Controller
{
    private $ts3;
    public function __construct()
    {
        try{
            $this->ts3 = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");
        }
        catch(Exception $e){
            return 'Unable to connect to server.';
        }
    }

    public function writeMessage(Request $request)
    {
        $this->ts3->message($request->input('message'));
        return response()->json(['success' => 'Message has been sent'], 200);
    }

    public function serverStatus() {
        $ping = intval($this->ts3->connectionInfo()['connection_ping']->__toString());
        $clients = $this->ts3->clientCount();
        $maxclients = $this->ts3->getProperty("virtualserver_maxclients");
        return response()->json(['ping' => $ping,'clients' => $clients, 'maxclients' => $maxclients], 200);
    }
}
```


For more information visit the [official documentation](https://docs.planetteamspeak.com/ts3/php/framework/).
