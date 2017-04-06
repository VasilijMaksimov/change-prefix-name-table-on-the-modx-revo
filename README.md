# change-prefix-name-table-on-the-modx-revo
change prefix-name-table on the modx revo
RU/
Код для смены префиксов баз данных MySql для безопасности сайта.
В поле $new_prefix = 'My_Prefix234_'; поставить своё название префиксов БД.
EN/
The code to change the prefix of MySql databases for the security of the site.
In box $new_prefix = 'My_Prefix234_'; put your name prefixes in the database.

<?php
ini_set("max_execution_time", 0);
ignore_user_abort(true);

$current_prefix = $modx->config['table_prefix'];

$new_prefix = 'My_Prefix234_';

$stmt = $modx->query("SHOW TABLES");
$tables = $stmt->fetchAll(PDO::FETCH_NUM);
$stmt->closeCursor();

foreach($tables as $table){
    
    $table = reset($table);
    
    $preg = "/^{$current_prefix}/u";
    
    if(preg_match($preg, $table)){
        $new_table_name = preg_replace($preg, $new_prefix, $table);
        $sql = "RENAME TABLE `{$table}` TO `{$new_table_name}`";
        if($s = $modx->prepare($sql)){
            $s->execute();
        }
    }
}

print "\nTables are updated";
