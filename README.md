# Rector - Instant Upgrades and Automated Refactoring
Rector instantly upgrades and refactors the PHP code of your application.

## Setup Environment
Create a `docker-compose-rector.yml` file in your `compose` directory
Content:
```bash
version: '3'
services:
  web_rector:
    image: ${REGISTRY_HOST}/${REGISTRY_REPOSITORY}/${REGISTRY_WEB_IMAGE}:${REGISTRY_WEB_TAG_VERSION}
    environment:
      PROJECT_ENV: $PROJECT_ENV
    volumes:
      - ../app/spgamepack.net:${PHP_APP_DIR}
```
Change Line #28 on `.env` file to setup `PHP 7.2` Image:
```bash
REGISTRY_WEB_TAG_VERSION=7.234.0-local
```
Run following command to get the image:
```bash
docker-compose -f ./docker-compose-rector.yml pull
```
Create and start containers on background:
```bash
docker-compose -f ./docker-compose-rector.yml up -d
```
Show list containers & result:
```bash
docker-compose -f ./docker-compose-rector.yml ps
tulgaa@DESKTOP-AL5KPVM:~/work/symfony-3-php55/compose$ docker-compose -f ./docker-compose-rector.yml ps
             Name                            Command               State   Ports
---------------------------------------------------------------------------------
spgamepack-compose_web_rector_1   docker-php-entrypoint /sta ...   Up      80/tcp
```

Show PHP version on a running container:
```bash
tulgaa@DESKTOP-AL5KPVM:~/work/symfony-3-php55/compose$ docker exec -it spgamepack-compose_web_rector_1 php -v
PHP 7.2.34 (cli) (built: Dec 11 2020 10:51:16) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2018 Zend Technologies
    with Zend OPcache v7.2.34, Copyright (c) 1999-2018, by Zend Technologies
```

## Install
```bash
composer require rector/rector --dev
```
Result:
```bash
Using version ^0.14.2 for rector/rector
./composer.json has been updated
Running composer update rector/rector
Loading composer repositories with package information
Updating dependencies
Lock file operations: 2 installs, 0 updates, 0 removals
  - Locking phpstan/phpstan (1.8.5)
  - Locking rector/rector (0.14.2)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 2 installs, 0 updates, 0 removals
  - Downloading phpstan/phpstan (1.8.5)
  - Installing phpstan/phpstan (1.8.5): Extracting archive
  - Installing rector/rector (0.14.2): Extracting archive
Package doctrine/doctrine-cache-bundle is abandoned, you should avoid using it. No replacement was suggested.
Package doctrine/reflection is abandoned, you should avoid using it. Use roave/better-reflection instead.
Package twig/extensions is abandoned, you should avoid using it. No replacement was suggested.
Package sensio/generator-bundle is abandoned, you should avoid using it. Use symfony/maker-bundle instead.
Generating autoload files
29 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
Class Sensio\Bundle\DistributionBundle\Composer\ScriptHandler is not autoloadable, can not call post-update-cmd script
No security vulnerability advisories found
```
## Runing Rector
Create a `rector.php` in your root directory:
```bash
vendor/bin/rector init
```

And modify it:
```bash
<?php

declare(strict_types=1);

use Rector\Config\RectorConfig;
use Rector\Set\ValueObject\LevelSetList;
use Rector\Core\ValueObject\PhpVersion;

return static function (RectorConfig $rectorConfig): void {
    $rectorConfig->paths([
        __DIR__ . '/src'
    ]);

    // define sets of rules
    $rectorConfig->sets([
        LevelSetList::UP_TO_PHP_55,
    ]);
    // use current php version is difference from the one refactor to.
    $rectorConfig->phpVersion(PhpVersion::PHP_55);
};
```
`$rectorConfig->paths` -> You can add directory to check by adding comma-separated.

`LevelSetList::UP_TO_PHP_55` -> Change to target PHP version

Then dry run Rector:
```bash
vendor/bin/rector process --dry-run --debug
```
Where:

  --dry-run -> Rector will show you diff of files that it *would* change
  
  --debug   -> To debug and show logs on terminal

Result:
```bash
root@97e2a0d5b579:/var/www/spgamepack.net# vendor/bin/rector process --dry-run
 81/81 [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓] 100%
5 files with changes
====================

1) src/Modern/StoreBundle/Repository/PointLogRepository.php:17

    ---------- begin diff ----------
@@ @@
         ->where('reason = 1')
         ->where('created_at >= :date')
         ->setParameter('member_id', $member_id)
-        ->setParameter('date', array(date('Y/m/d 00:00:00')))
+        ->setParameter('date', [date('Y/m/d 00:00:00')])
         ->getQuery();
-      $count = count($q->getResult());
+      $count = is_array($q->getResult()) || $q->getResult() instanceof \Countable ? count($q->getResult()) : 0;
     }
     return ($count) ? (TRUE) : (FALSE);
   }
    ----------- end diff -----------

Applied rules:
 * LongArrayToShortArrayRector
 * CountOnNullRector (https://3v4l.org/Bndc9)


2) src/Modern/StoreBundle/Controller/DefaultController.php:7

    ---------- begin diff ----------
@@ @@
 {
     public function indexAction($name)
     {
-        return $this->render('ModernStoreBundle:Default:index.html.twig', array('name' => $name));
+        return $this->render('ModernStoreBundle:Default:index.html.twig', ['name' => $name]);
     }
 }
    ----------- end diff -----------

Applied rules:
 * LongArrayToShortArrayRector


3) src/Modern/SmartphoneBundle/EventListener/LoginFilterListener.php:34

    ---------- begin diff ----------
@@ @@
       // $controller[0] -> get('logger')->info('=================== uid: ' . $uid . ' ===================');

       //広告ID
-      $ad_params = array();
+      $ad_params = [];

       # -------------------------------------------------
       # リワード
    ----------- end diff -----------

Applied rules:
 * LongArrayToShortArrayRector


4) src/Modern/SmartphoneBundle/Controller/InfoController.php:16

    ---------- begin diff ----------
@@ @@
     $logger = $this->get('logger');
     $logger->info('=================== top_link: ' . $top_link . ' ===================');

-    return $this->render('ModernSmartphoneBundle:Info:index.html.twig', array(
-      '_SERVER' => $_SERVER,
-      'top_link' => $top_link,
-    ));
+    return $this->render('ModernSmartphoneBundle:Info:index.html.twig', ['_SERVER' => $_SERVER, 'top_link' => $top_link]);
   }

   public function tamekataAction(Request $request)
@@ @@
     // $logger = $this->get('logger');
     // $logger->info('=================== member: ' . $member->getName() . ' ===================');

-    return $this->render('ModernSmartphoneBundle:Info:tamekataSuccess.html.twig', array(
-      'top_link' => $top_link,
-      '_SERVER' => $_SERVER,
-    ));
+    return $this->render('ModernSmartphoneBundle:Info:tamekataSuccess.html.twig', ['top_link' => $top_link, '_SERVER' => $_SERVER]);
   }
 }
    ----------- end diff -----------

Applied rules:
 * LongArrayToShortArrayRector


5) src/Modern/SmartphoneBundle/Controller/IndexController.php:9

    ---------- begin diff ----------
@@ @@
 {
   public function indexAction(Request $request)
   {
+    $delimiter = null;
     $this->member_id = $request->query->get("member_id");
     mcrypt_get_iv_size();
-    $callable = create_function('$matches', "return '$delimiter' . strtolower(\$matches[1]);");
+    $callable = function ($matches) use ($delimiter) {
+        return $delimiter . strtolower($matches[1]);
+    };
     // $logger = $this->get('logger');
     // $logger->info('=================== ' . sizeof($request->query->all()) . ' ===================');

@@ @@
       // Mopo導入仕様書ver1.5.pdfに記載の方法で成果通知
       try {
         $url = "https://af.mopo.jp/te/action";
-        $data_arr = array("c" => "142635", "x" => "5967", "mopoa" => $mopoa, "t_id" => $sugotokuIdHash);
+        $data_arr = ["c" => "142635", "x" => "5967", "mopoa" => $mopoa, "t_id" => $sugotokuIdHash];
         $option = [
           CURLOPT_RETURNTRANSFER => true, //レスポンスを標準出力に出すのではなく文字列として返す
           CURLOPT_TIMEOUT => 5, // タイムアウト時間
@@ @@

     // $this->oneDayRanking = $em->getRepository('ModernStoreBundle:OneDayRanking')->getRank($open_at = date("Y-m-d"),$limit=10);

-    return $this->render('ModernSmartphoneBundle:Index:index.html.twig', array(
-      'todayGame' => $this->todayGame,
-      '_SERVER' => $_SERVER,
-      'PointFirstTime' => isset($this->PointFirstTime) ? $this->PointFirstTime : FALSE,
-      'point' => isset($this->point) ? $this->point : null,
-      'games' => $this->games,
-      'x_favorite_flg' => isset($headers['x-favorite-flg']) ? $headers['x-favorite-flg'] : '',
-      'docomo_link' => $this->docomo_link,
-      'is_managed_ip' => $this->is_managed_ip ? '?' : '&',
-      'uid' => $uid,
-      'medal1' => $medal1,
-      'medal2' => $medal2,
-      'medal3' => $medal3,
-      'medal4' => $medal4,
-      'gameid' => $gameid,
-      'my_total_point' => $this->my_total_point,
-      // 'oneDayRanking' => $this->oneDayRanking,
-    ));
+    return $this->render('ModernSmartphoneBundle:Index:index.html.twig', ['todayGame' => $this->todayGame, '_SERVER' => $_SERVER, 'PointFirstTime' => $this->PointFirstTime ?? FALSE, 'point' => $this->point ?? null, 'games' => $this->games, 'x_favorite_flg' => $headers['x-favorite-flg'] ?? '', 'docomo_link' => $this->docomo_link, 'is_managed_ip' => $this->is_managed_ip ? '?' : '&', 'uid' => $uid, 'medal1' => $medal1, 'medal2' => $medal2, 'medal3' => $medal3, 'medal4' => $medal4, 'gameid' => $gameid, 'my_total_point' => $this->my_total_point]);
   }
 }
    ----------- end diff -----------

Applied rules:
 * LongArrayToShortArrayRector
 * AddDefaultValueForUndefinedVariableRector (https://github.com/vimeo/psalm/blob/29b70442b11e3e66113935a2ee22e165a70c74a4/docs/fixing_code.md#possiblyundefinedvariable)
 * TernaryToNullCoalescingRector
 * CreateFunctionToAnonymousFunctionRector (https://stackoverflow.com/q/48161526/1348344 http://php.net/manual/en/migration72.deprecated.php#migration72.deprecated.create_function-function)



 [OK] 5 files would have changed (dry-run) by Rector


root@97e2a0d5b579:/var/www/spgamepack.net#
```
To *make* the changes, drop `--dry-run`:
```bash
vendor/bin/rector process
```
