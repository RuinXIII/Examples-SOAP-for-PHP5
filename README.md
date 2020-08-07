# Examples-SOAP-for-PHP5
An example implementation of the VoIP.ms API using PHP5

# Documentation
The Voip.ms API documentation can be found in the [Voip.ms Customer Portal](https://www.voip.ms/m/apidocs.php)

# Getting Started
You will have to enable API access from your account in the Customer Portal at Main Menu > SOAP and REST/JSON API or directly [here](https://www.voip.ms/m/api.php). 

In the same section, you can setup your API password and you'll have to add the IP address where the API is going to be used from. You can also use 0.0.0.0 as a wildcard, but this is discouraged for safety reasons.

![API Interface](https://i.imgur.com/aFEDkvg.png "API Interface")

Your API credentials need to be declared inside the code for the functions to work properly.

```PHP
var $api_username   = 'john.doe@mydomain.com';
var $api_password   = 'johnspassword';

    var $soap_client;
    function soapClient(){
        $this->soap_client = new nusoap_client("https://voip.ms/api/v1/server.php");
    }

    function soapCall($function, $params){
        if(!$this->soap_client){$this->soapClient();}
        return $this->soap_client->call($function, $params);
    }

```

# Get Registration Status

```PHP
/* Account */
$account = "100000_VoIP";

/* Get Registration Status */
$response = $voipms->getRegistrationStatus($account);

/* Get Errors - Invalid_Account */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Is Registered */
echo "{$account} Registered : {$response[registered]}";

/* Get Registrations Array */
$registrations = $response[registrations];

/* Display Clients */
foreach($registrations as $registration){
    ?>
    <table style="border: 1px solid #336699; width:500px">
        <tr>
            <td>Server Name:</td>
            <td>
                <? echo $registration[server_name] ?>
            </td>
        <tr>
        
        <tr>
            <td>Server Short Name:</td>
            <td>
                <? echo $registration[server_shortname] ?>
            </td>
        <tr>
        
        <tr>
            <td>Server Hostame:</td>
            <td>
                <? echo $registration[server_hostname] ?>
            </td>
        <tr>
        
        <tr>
            <td>Server IP:</td>
            <td>
                <? echo $registration[server_ip] ?>
            </td>
        <tr>
        
        <tr>
            <td>Server Country:</td>
            <td>
                <? echo $registration[server_country] ?>
            </td>
        <tr>
        
        <tr>
            <td>Server POP:</td>
            <td>
                <? echo $registration[server_pop] ?>
            </td>
        <tr>
        
        <tr>
            <td>Register IP:</td>
            <td>
                <? echo $registration[register_ip] ?>
            </td>
        <tr>
        
        <tr>
            <td>Register Port:</td>
            <td>
                <? echo $registration[register_port] ?>
            </td>
        <tr>
        
        <tr>
            <td>Next Registration:</td>
            <td>
                <? echo $registration[register_next] ?>
            </td>
        <tr>
       
    </table>
    <br><br><br>
    <?
}
```

# Client Login

```PHP
/* Client Email and Password */
$client = "500000";
$email = "jane.doe@mydomain.com";
$password = "janespassword";

/* Get Client by Email */
$response = $voipms->getClients($email);

/* You can also get clients by ClientNumber */
//$response = $voipms->getClients($client);

/* Get Errors - Invalid_Client */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* See if Password is Correct */
$client = $response[clients][0];
if($password!=$client[password]){
    echo "invalid_password";
    exit;
}

/* Client Exists and Password OK */
echo "{$client[client]} - {$client[firstname]} {$client[lastname]}"
```

# Get Client Accounts

```PHP
/* ClientNumber / Username / Account */
$client = "500000";
$account = "100000_VoIP";

/* Get Accounts by Client Number */
$response = $voipms->getSubAccounts($client);

/* You can also get Accounts by Account */
//$response = $voipms->getSubAccounts($account);

/* You can get all your Accounts by passing no param */
//$response = $voipms->getSubAccounts();

/* Get Errors - Invalid_Account */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Get Accounts Array */
$accounts = $response[accounts];

/* Display Accounts */
foreach($accounts as $account){
    ?>
    <table style="border: 1px solid #336699; width:500px">
        <tr>
            <td>Account:</td>
            <td>
                <? echo $account[account] ?>
            </td>
        <tr>
        
        <tr>
            <td>Username:</td>
            <td>
                <? echo $account[username] ?>
            </td>
        <tr>
        
        <tr>
            <td>Description:</td>
            <td>
                <? echo $account[description] ?>
            </td>
        <tr>
        
        <tr>
            <td>Protocol:</td>
            <td>
                <? 
                    $response = $voipms->getProtocols($account[protocol]);
                    echo $response[protocols][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Authorizacion Type:</td>
            <td>
                <? 
                    $response = $voipms->getAuthTypes($account[auth_type]);
                    echo $response[auth_types][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Password:</td>
            <td>
                <? echo $account[password] ?>
            </td>
        <tr>
        
        <tr>
            <td>IP:</td>
            <td>
                <? echo $account[ip] ?>
            </td>
        <tr>
        
        <tr>
            <td>Device Type:</td>
            <td>
                <? 
                    $response = $voipms->getDeviceTypes($account[device_type]);
                    echo $response[device_types][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>CallerID Number:</td>
            <td>
                <? echo $account[callerid_number] ?>
            </td>
        <tr>
        
        <tr>
            <td>Allow International Calls:</td>
            <td>
                <? 
                    $response = $voipms->getLockInternational($account[lock_international]);
                    echo $response[lock_international][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>International Route:</td>
            <td>
                <? 
                    $response = $voipms->getRoutes($account[international_route]);
                    echo $response[routes][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Music on Hold:</td>
            <td>
                <? 
                    $response = $voipms->getMusicOnHold($account[music_on_hold]);
                    echo $response[music_on_hold][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Allowed Codecs:</td>
            <td>
                <? 
                    $display = "";
                    $codecs = explode(";",$account[allowed_codecs]);
                    foreach($codecs as $codec){
                        $response = $voipms->getAllowedCodecs($codec);
                        if($display){$display .= ", ";}
                        $display .= $response[allowed_codecs][0][description];
                    }
                    echo $display;
                    
                ?>
            </td>
        <tr>
        
        <tr>
            <td>DTMF Mode:</td>
            <td>
                <? 
                    $response = $voipms->getDTMFModes($account[dtmf_mode]);
                    echo $response[dtmf_modes][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>NAT:</td>
            <td>
                <? 
                    $response = $voipms->getNAT($account[nat]);
                    echo $response[nat][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Internal Extension:</td>
            <td>
                <? echo $account[internal_extension]; ?>
            </td>
        <tr>
        
        <tr>
            <td>Internal Voicemail:</td>
            <td>
                <? 
                    $response = $voipms->getVoicemails($account[internal_voicemail]);
                    echo "{$response[voicemails][0][mailbox]} - {$response[voicemails][0][name]}";
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Internal Dialtime:</td>
            <td>
                <? echo $account[internal_dialtime]; ?>
            </td>
        <tr>
        
        <tr>
            <td>Reseller Client:</td>
            <td>
                <? 
                    $response = $voipms->getClients($account[reseller_client]);
                    echo "{$response[clients][0][client]} - {$response[clients][0][email]}";
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Reseller Package:</td>
            <td>
                <? 
                    $response = $voipms->getPackages($account[reseller_package]);
                    echo "{$response[packages][0][package]} - {$response[packages][0][name]}";
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Reseller Next Billing Date:</td>
            <td>
                <? echo $account[reseller_nextbilling]; ?>
            </td>
        <tr>
        
    </table>
    <br><br><br>
    <?
}
```

# Voicemails

```PHP
/* Mailbox Number */
$mailbox = "101";

/* Get Voicemail by Mailbox Number */
$response = $voipms->getVoicemails($mailbox);

/* You can also get all your Voicemails by passing no param */
//$response = $voipms->getVoicemails();

/* Get Errors - Invalid_Voicemail */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Get Voicemails Array */
$voicemails = $response[voicemails];

/* Display Voicemails */
foreach($voicemails as $voicemail){
    ?>
    <table style="border: 1px solid #336699; width:500px">
        <tr>
            <td>Mailbox:</td>
            <td>
                <? echo $voicemail[mailbox] ?>
            </td>
        <tr>
        
        <tr>
            <td>Name:</td>
            <td>
                <? echo $voicemail[name] ?>
            </td>
        <tr>
        
        
        <tr>
            <td>Password:</td>
            <td>
                <? echo $voicemail[password] ?>
            </td>
        <tr>
        
        <tr>
            <td>Skip Password:</td>
            <td>
                <? echo $voicemail[skip_password]?"Yes":"No" ?>
            </td>
        <tr>
        
        <tr>
            <td>e-mail:</td>
            <td>
                <? echo $voicemail[email] ?>
            </td>
        <tr>
        
        <tr>
            <td>Attach Message to e-mail:</td>
            <td>
                <? echo $voicemail[attach_message] ?>
            </td>
        <tr>
        
        <tr>
            <td>Delete Message after emailed:</td>
            <td>
                <? echo $voicemail[delete_message] ?>
            </td>
        <tr>
        
        <tr>
            <td>Say Time Envelope:</td>
            <td>
                <? echo $voicemail[say_time] ?>
            </td>
        <tr>
        
        
        <tr>
            <td>Time Zone:</td>
            <td>
                <? 
                    $response = $voipms->getTimeZones($voicemail[timezone]);
                    echo $response[timezones][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Say Caller ID:</td>
            <td>
                <? echo $voicemail[say_callerid] ?>
            </td>
        <tr>
        
        <tr>
            <td>Play Instructions before beep:</td>
            <td>
                <? 
                    $response = $voipms->getPlayInstructions($voicemail[play_instructions]);
                    echo $response[play_instructions][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Language:</td>
            <td>
                <? 
                    $response = $voipms->getLanguages($voicemail[language]);
                    echo $response[languages][0][description];
                ?>
            </td>
        <tr>
       
    </table>
    <br><br><br>
    <?
}
```

# Client Management

```PHP
/* ClientNumber / Password */
$client = "500000";
$email = "jane.doe@mydomain.com";

/* Get Client by Email */
//$response = $voipms->getClients($email);

/* You can also get clients by ClientNumber */
//$response = $voipms->getClients($client);

/* You can get all your clients by passing no param */
$response = $voipms->getClients();


/* Get Errors - Invalid_Client */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Get Clients Array */
$clients = $response[clients];

/* Display Clients */
foreach($clients as $client){
    ?>
    <table style="border: 1px solid #336699; width:500px">
        <tr>
            <td>Client:</td>
            <td>
                <? echo $client[client] ?>
            </td>
        <tr>
        
        <tr>
            <td>Email:</td>
            <td>
                <? echo $client[email] ?>
            </td>
        <tr>
        
        <tr>
            <td>Password:</td>
            <td>
                <? echo $client[password] ?>
            </td>
        <tr>
        
        <tr>
            <td>Company:</td>
            <td>
                <? echo $client[company] ?>
            </td>
        <tr>
        
        <tr>
            <td>Firstname:</td>
            <td>
                <? echo $client[firstname] ?>
            </td>
        <tr>
        
        <tr>
            <td>Lastname:</td>
            <td>
                <? echo $client[lastname] ?>
            </td>
        <tr>
        
        <tr>
            <td>Address:</td>
            <td>
                <? echo $client[address] ?>
            </td>
        <tr>
        
        <tr>
            <td>City:</td>
            <td>
                <? echo $client[city] ?>
            </td>
        <tr>
        
        <tr>
            <td>State:</td>
            <td>
                <? echo $client[state] ?>
            </td>
        <tr>
        
        <tr>
            <td>Country:</td>
            <td>
                <? echo $client[country] ?>
            </td>
        <tr>
        
        <tr>
            <td>Zip:</td>
            <td>
                <? echo $client[zip] ?>
            </td>
        <tr>
        
        <tr>
            <td>Phone Number:</td>
            <td>
                <? echo $client[phone_number] ?>
            </td>
        <tr>
        
        <tr>
            <td>Balance Management:</td>
            <td>
                <? 
                    $response = $voipms->getBalanceManagement($client[balance_management]);
                    echo $response[balance_management][0][description];
                ?>
            </td>
        <tr>
       
    </table>
    <br><br><br>
    <?
}
```

# Get Packages

```PHP
/* PackageNumber */
$package = "1001";

/* Get Package by Package Number */
//$response = $voipms->getPackages($package);

/* You can get all your Packages by passing no param */
$response = $voipms->getPackages();


/* Get Errors - Invalid_Package */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Get Packages Array */
$packages = $response[packages];

/* Display Packages */
foreach($packages as $package){
    ?>
    <table style="border: 1px solid #336699; width:500px">
        <tr>
            <td>Package:</td>
            <td>
                <? echo $package[package] ?>
            </td>
        <tr>
        
        <tr>
            <td>Name:</td>
            <td>
                <? echo $package[name] ?>
            </td>
        <tr>
        
        <tr>
            <td>Mark Up Fixed:</td>
            <td>
                <? echo $package[markup_fixed] ?>
            </td>
        <tr>
        
        <tr>
            <td>Mark Up Percentage:</td>
            <td>
                <? echo $package[markup_percentage] ?>
            </td>
        <tr>
        
        <tr>
            <td>Pulse:</td>
            <td>
                <? echo $package[pulse] ?>
            </td>
        <tr>
        
        <tr>
            <td>International Route:</td>
            <td>
                <? 
                    $response = $voipms->getRoutes($package[international_route]);
                    echo $response[routes][0][description];
                ?>
            </td>
        <tr>
        
        <tr>
            <td>Montly Fee:</td>
            <td>
                <? echo $package[monthly_fee] ?>
            </td>
        <tr>
        
        <tr>
            <td>Setup Fee:</td>
            <td>
                <? echo $package[setup_fee] ?>
            </td>
        <tr>
        
        <tr>
            <td>Free Minutes:</td>
            <td>
                <? echo $package[free_minutes] ?>
            </td>
        <tr>
       
    </table>
    <br><br><br>
    <?
}
```

# Call Detail Records

```PHP
/* Get CDR */
$response = $voipms->getCDR('2010-11-10','2010-11-10',1,1,1,1,-5,'all','all','all');

/* Get Errors - no_cdr */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Get CDR Array */
$cdr = $response[cdr];

/* Calls */
$calls = count($cdr);

/* Duration / Total */
foreach($cdr as $call){
    $duration += $call[seconds];
    $total += $call[total];
}
$duration = secToTime($duration);

?>
<table cellspacing="0" cellpadding="6" style="border: 1px solid #336699">
    <tr style="border: 1px solid #336699">
        <td style="border: 1px solid #336699">
            Date
        </td>
        <td style="border: 1px solid #336699">
            CallerID
        </td>
        <td style="border: 1px solid #336699">
            Destination
        </td>
        <td style="border: 1px solid #336699">
            Description
        </td>
        <td style="border: 1px solid #336699">
            Duration
        </td>
        <td style="border: 1px solid #336699">
            Rate
        </td>
        <td style="border: 1px solid #336699">
            Total
        </td>
    
    </tr>
    <?
    foreach($cdr as $call){
        ?>
        <tr>
            <td>
                <? echo $call[date]; ?>
            </td>
            <td>
                <? echo $call[callerid]; ?>
            </td>
            <td>
                <? echo $call[destination]; ?>
            </td>
            <td>
                <? echo $call[description]; ?>
            </td>
            <td align="right">
                <? echo $call[duration]; ?>
            </td>
            <td align="right">
                <? echo $call[rate]; ?>
            </td>
            <td align="right">
                <? echo $call[total]; ?>
            </td>
        </tr>
        <?
    }
    ?>
    <tr>
        <td colspan="7" align="right">
            <? echo "Calls: {$calls} | Duration: {$duration} | Total: \${$total}"; ?>
        </td>
    </tr>
</table>

<?

/* Function to Calculate Time */
function secToTime($secs){
    if(!$secs){return 0;}
    
    $vals = array('h' => floor($secs / 3600), 
                  'm' => floor($secs % 3600 / 60), 
                  's' => $secs % 60); 

    $ret = array(); 

    $added = false; 
    foreach ($vals as $k => $v) { 
        if ($v > 0 || $added) { 
            $ret[] = ((strlen($v) == 1) && $added) ? "0$v" : $v; 
            $added = true;
        } 
    } 

    return join(':', $ret); 
}
```

# CDR Reseller

```PHP
$client = 500000;

/* Get CDR */
$response = $voipms->getResellerCDR('2010-11-10','2010-11-10', $client ,1,1,1,1,-5,'all','all','all');

/* Get Errors - no_cdr */
if($response[status]!='success'){
    echo $response[status];
    exit;
}

/* Get CDR Array */
$cdr = $response[cdr];

/* Calls */
$calls = count($cdr);

/* Duration / Total */
foreach($cdr as $call){
    $duration += $call[seconds];
    $total += $call[total];
}
$duration = secToTime($duration);

?>
<table cellspacing="0" cellpadding="6" style="border: 1px solid #336699">
    <tr style="border: 1px solid #336699">
        <td style="border: 1px solid #336699">
            Date
        </td>
        <td style="border: 1px solid #336699">
            CallerID
        </td>
        <td style="border: 1px solid #336699">
            Destination
        </td>
        <td style="border: 1px solid #336699">
            Description
        </td>
        <td style="border: 1px solid #336699">
            Duration
        </td>
        <td style="border: 1px solid #336699">
            Cost
        </td>
    
    </tr>
    <?
    foreach($cdr as $call){
        ?>
        <tr>
            <td>
                <? echo $call[date]; ?>
            </td>
            <td>
                <? echo $call[callerid]; ?>
            </td>
            <td>
                <? echo $call[destination]; ?>
            </td>
            <td>
                <? echo $call[description]; ?>
            </td>
            <td align="right">
                <? echo $call[duration]; ?>
            </td>
            <td align="right">
                <? echo $call[total]; ?>
            </td>
        </tr>
        <?
    }
    ?>
    <tr>
        <td colspan="7" align="right">
            <? echo "Calls: {$calls} | Duration: {$duration} | Total: \${$total}"; ?>
        </td>
    </tr>
</table>

<?

/* Function to Calculate Time */
function secToTime($secs){
    if(!$secs){return 0;}
    
    $vals = array('h' => floor($secs / 3600), 
                  'm' => floor($secs % 3600 / 60), 
                  's' => $secs % 60); 

    $ret = array(); 

    $added = false; 
    foreach ($vals as $k => $v) { 
        if ($v > 0 || $added) { 
            $ret[] = ((strlen($v) == 1) && $added) ? "0$v" : $v; 
            $added = true;
        } 
    } 

    return join(':', $ret); 
}
```

# Support

Please note that we do NOT provide support with your programming language. The only support provided by our staff is for bugs, documentation errors, documentation missing or other questions regarding the functionality of the API. Support regarding the API will be addressed via the ticketing system only at [support@voip.ms](support@voip.ms). Our programmers do not have access to the Live Chat. 
