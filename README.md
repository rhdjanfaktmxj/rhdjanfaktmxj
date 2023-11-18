<?php
function get_client_ip() {
    $ipaddress = '';
    if (getenv('HTTP_CLIENT_IP'))
        $ipaddress = getenv('HTTP_CLIENT_IP');
    else if(getenv('HTTP_X_FORWARDED_FOR'))
        $ipaddress = getenv('HTTP_X_FORWARDED_FOR');
    else if(getenv('HTTP_X_FORWARDED'))
        $ipaddress = getenv('HTTP_X_FORWARDED');
    else if(getenv('HTTP_FORWARDED_FOR'))
        $ipaddress = getenv('HTTP_FORWARDED_FOR');
    else if(getenv('HTTP_FORWARDED'))
        $ipaddress = getenv('HTTP_FORWARDED');
    else if(getenv('REMOTE_ADDR'))
        $ipaddress = getenv('REMOTE_ADDR');
    else
        $ipaddress = 'UNKNOWN';
    return $ipaddress;
}

function getBrowserInfo() 
{
  $userAgent = $_SERVER["HTTP_USER_AGENT"]; 
  if(preg_match('/MSIE/i',$userAgent) && !preg_match('/Opera/i',$u_agent)){
    $browser = 'Internet Explorer';
  }
  else if(preg_match('/Firefox/i',$userAgent)){
    $browser = 'Mozilla Firefox';
  }
  else if (preg_match('/Chrome/i',$userAgent)){
    $browser = 'Google Chrome';
  }
  else if(preg_match('/Safari/i',$userAgent)){
    $browser = 'Apple Safari';
  }
  elseif(preg_match('/Opera/i',$userAgent)){
    $browser = 'Opera';
  }
  elseif(preg_match('/Netscape/i',$userAgent)){
    $browser = 'Netscape';
  }
  else{
    $browser = "Other";
  }

  return $browser;
}

function getOsInfo()
{
  $userAgent = $_SERVER["HTTP_USER_AGENT"]; 

  if (preg_match('/linux/i', $userAgent)){ 
    $os = 'linux';}
  elseif(preg_match('/macintosh|mac os x/i', $userAgent)){
    $os = 'mac';}
  elseif (preg_match('/windows|win32/i', $userAgent)){
    $os = 'windows';}
  else {
    $os = 'Other';

  }

  return $os;
}

function IsHttps() {
  return (!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') || $_SERVER['SERVER_PORT'] == 443;
}

$http = IsHttps();

$os = getOsInfo();

$ip = get_client_ip();

$Browser = getBrowserInfo();

$file = fopen("ip.txt", "a");

$date = date("Y-m-d H:i:s");

echo $os;

echo "<br>";

echo $Browser;

echo "<br>";

echo $ip;

echo "<br>";

echo $date;

echo "<br>";

echo $http;

fwrite($file, "접속자의 ip : ".$ip."     ");
fwrite($file, "접속자의 브라우저 : ".$Browser."     ");
fwrite($file, "접속자의 os : ".$os."\n");
fwrite($file, "http, https 여부. 만약 https라면 1 : ".$http."     ");
fwrite($file, "접속자의 접속 날짜와 시, 분 : ".$date."\n"."\n");
fclose($file);

?>
