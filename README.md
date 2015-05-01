<?php
// PHP Скрипт бесплатной панели Vesta (vestacp.com) позволяет через обычные запросы в браузере добавлять, смотреть, удалять домены и пользователей.
// Небольшой хелп
// Заменяем 55.66.77.88 на свой IP где установлена панель
// $vst_password = 'passpass'; - заменяем на свой пароль суперпользователя (admin).

// Команды :
// http://55.66.77.88/index.php?vst_command=v-add-domain&username=user300415&domain=addomen.com   - Добавляем домены
// http://55.66.77.88/index.php?vst_command=v-delete-domain&username=user300415&domain=addomen.com   - Удаляем домены
// http://55.66.77.88/index.php?vst_command=v-list-web-domains&username=user300415   - Показываем домены
// http://55.66.77.88/index.php?vst_command=v-add-user&username=user300415&pass=passpass  - Добавляем пользователя
// http://55.66.77.88/index.php?vst_command=v-delete-user&username=user300415  - Удаляем пользователя
// http://55.66.77.88/index.php?vst_command=v-list-users  - Показываем пользователей


// Server credentials
$vst_hostname = '55.66.77.88';
$vst_username = 'admin'; 
$vst_password = 'passpass';
$vst_returncode = 'yes';
$vst_command = $_GET['vst_command'];
if ($vst_command !='v-list-users' or $vst_command =='v-add-user' or $vst_command =='v-delete-user') {$username = $_GET['username'];}
if ($vst_command =='v-add-domain' or $vst_command =='v-delete-domain') {$domain = $_GET['domain'];}




if ($vst_command =='v-add-domain') {
// Prepare POST query
$postvars = array(
    'user' => $vst_username,
    'password' => $vst_password,
    'returncode' => $vst_returncode,
    'cmd' => $vst_command,
    'arg1' => $username,
    'arg2' => $domain
);
}

if ($vst_command =='v-delete-domain') {
// Prepare POST query
$postvars = array(
    'user' => $vst_username,
    'password' => $vst_password,
    'returncode' => $vst_returncode,
    'cmd' => $vst_command,
    'arg1' => $username,
    'arg2' => $domain
);
}


if ($vst_command =='v-list-web-domains') {

$format = 'json';    
// Prepare POST query
$postvars = array(
    'user' => $vst_username,
    'password' => $vst_password,
    'cmd' => $vst_command,
    'arg1' => $username,
    'arg2' => $format
);
}


if ($vst_command =='v-list-users') {

$format = 'json';    
// Prepare POST query
$postvars = array(
    'user' => $vst_username,
    'password' => $vst_password,
    'cmd' => $vst_command,
    'arg1' => $format
);
}


if ($vst_command =='v-add-user') {

$email = 'admin@gmail.com';
$package = 'default';
$fist_name = 'Anton';
$last_name = 'Gar';
// Prepare POST query
$postvars = array(
    'user' => $vst_username,
    'password' => $vst_password,
    'returncode' => $vst_returncode,
    'cmd' => $vst_command,
    'arg1' => $username,
    'arg2' => $_GET['pass'],
    'arg3' => $email,
    'arg4' => $package,
    'arg5' => $fist_name,
    'arg6' => $last_name
);
}


if ($vst_command =='v-delete-user') {
// Prepare POST query
$postvars = array(
    'user' => $vst_username,
    'password' => $vst_password,
    'returncode' => $vst_returncode,
    'cmd' => $vst_command,
    'arg1' => $username
);
}

// ОБЩИЙ БЛОК
$postdata = http_build_query($postvars);

// Send POST query via cURL
$postdata = http_build_query($postvars);
$curl = curl_init();
curl_setopt($curl, CURLOPT_URL, 'https://' . $vst_hostname . ':8083/api/');
curl_setopt($curl, CURLOPT_RETURNTRANSFER,true);
curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, false);
curl_setopt($curl, CURLOPT_POST, true);
curl_setopt($curl, CURLOPT_POSTFIELDS, $postdata);
$answer = curl_exec($curl);



if ($vst_command =='v-add-domain'  or $vst_command =='v-delete-domain'  or $vst_command =='v-add-user') {
// Check result
if($answer == 0) {
    echo "Successfuly\n";
} else {
    echo "Query returned error code: " .$answer. "\n";
}
}


if ($vst_command =='v-list-web-domains' or  $vst_command =='v-list-users') {

// Parse JSON output
$data = json_decode($answer, true);

// Print result
//print_r(var_dump($data);

echo join('<br>', array_keys($data));

}


?>
