[![SensioLabsInsight](https://insight.sensiolabs.com/projects/0a685157-ea99-4554-b302-9e8879d05648/small.png)](https://insight.sensiolabs.com/projects/0a685157-ea99-4554-b302-9e8879d05648)
[![Build Status](https://travis-ci.org/odenktools/php-bca.svg?branch=master)](https://travis-ci.org/odenktools/php-bca)
[![codecov](https://codecov.io/gh/odenktools/php-bca/branch/master/graph/badge.svg)](https://codecov.io/gh/odenktools/php-bca)
[![Latest Stable Version](https://poser.pugx.org/odenktools/php-bca/v/stable)](https://packagist.org/packages/odenktools/php-bca)
[![Latest Unstable Version](https://poser.pugx.org/odenktools/php-bca/v/unstable)](https://packagist.org/packages/odenktools/php-bca)

# BCA (Bank Central Asia)

Native PHP library untuk mengintegrasikan Aplikasi Anda dengan sistem BCA (Bank Central Asia). Untuk dokumentasi lebih jelas dan lengkap, silahkan kunjungi website resminya di [Developer BCA](https://developer.bca.co.id/documentation).

Untuk Framework ```Laravel``` bisa menggunakan library [Odenktools Laravel BCA](https://github.com/odenktools/laravel-bca).

## Fitur Library

* [Installasi](https://github.com/odenktools/php-bca#instalasi)
* [Setting](https://github.com/odenktools/php-bca#koneksi-dan-setting)
* [Login](https://github.com/odenktools/php-bca#login)
* [Informasi Saldo](https://github.com/odenktools/php-bca#balance-information)
* [Transfer](https://github.com/odenktools/php-bca#fund-transfer)
* [Mutasi Rekening](https://github.com/odenktools/php-bca#account-statement)
* [Info Kurs](https://github.com/odenktools/php-bca#foreign-exchange-rate)
* [Pencarian ATM Terdekat](https://github.com/odenktools/php-bca#nearest-atm-locator)
* [Deposit Rate](https://github.com/odenktools/php-bca#deposit-rate)

### INSTALASI

```bash
composer require "odenktools/php-bca"
```

### KONEKSI DAN SETTING

Sebelum masuk ke tahap ```LOGIN``` pastikan seluruh kebutuhan seperti ```CORP_ID, CLIENT_KEY, CLIENT_SECRET, APIKEY, SECRETKEY``` telah diketahui.

```php
    $options = array(
        'scheme'        => 'https',
        'port'          => 443,
        'host'          => 'sandbox.bca.co.id',
        'timezone'      => 'Asia/Jakarta',
        'timeout'       => 30,
        'debug'         => true,
        'development'   => true
    );

	// Setting default timezone Anda
	\Bca\BcaHttp::setTimeZone('Asia/Jakarta');

	// ATAU
	
	// \Bca\BcaHttp::setTimeZone('Asia/Singapore');

	$corp_id = "BCAAPI2016";
	$client_key = "NILAI-CLIENT-KEY-ANDA";
	$client_secret = "NILAI-CLIENT-SECRET-ANDA";
	$apikey = "NILAI-APIKEY-ANDA";
	$secret = "SECRETKEY-ANDA";

	$bca = new \Bca\BcaHttp($corp_id, $client_key, $client_secret, $apikey, $secret);

	// ATAU

	$bca = new \Bca\BcaHttp($corp_id, $client_key, $client_secret, $apikey, $secret, $options);
```

### LOGIN

```php
	$corp_id = "CORP_ID-ANDA";
	$client_key = "NILAI-CLIENT-KEY-ANDA";
	$client_secret = "NILAI-CLIENT-SECRET-ANDA";
	$apikey = "NILAI-APIKEY-ANDA";
	$secret = "SECRETKEY-ANDA";

	$bca = new \Bca\BcaHttp($corp_id, $client_key, $client_secret, $apikey, $secret);

	// Request Login dan dapatkan nilai OAUTH
	$response = $bca->httpAuth();

	// LIHAT HASIL OUTPUT
	echo json_encode($response);
```

Setelah Login berhasil pastikan anda menyimpan nilai ```TOKEN``` di tempat yang aman, karena nilai ```TOKEN``` tersebut agar digunakan untuk tugas tugas berikutnya.

### BALANCE INFORMATION

Pastikan anda mendapatkan nilai ```TOKEN``` dan ```TOKEN``` tersebut masih berlaku (Tidak Expired).

```php
	// Nilai token yang dihasilkan saat login
	$token = "MvXPqa5bQs5U09Bbn8uejBE79BjI3NNCwXrtMnjdu52heeZmw9oXgB";

	//Nomor akun yang akan di ambil informasi saldonya, menggunakan ARRAY
	$arrayAccNumber = array('0201245680', '0063001004', '1111111111');

	$response = $bca->getBalanceInfo($token, $arrayAccNumber);
	
	// LIHAT HASIL OUTPUT
	echo json_encode($response);
```

### FUND TRANSFER

Pastikan anda mendapatkan nilai ```TOKEN``` dan ```TOKEN``` tersebut masih berlaku (Tidak Expired).

```php
	// Nilai token yang dihasilkan saat login
	$token = "MvXPqa5bQs5U09Bbn8uejBE79BjI3NNCwXrtMnjdu52heeZmw9oXgB";

	$amount = '50000.00';

	// Nilai akun bank anda
	$nomorakun = '0201245680';

	// Nilai akun bank yang akan ditransfer
	$nomordestinasi = '0201245681';

	// Nomor PO, silahkan sesuaikan
	$nomorPO = '12345/PO/2017';

	// Nomor Transaksi anda, Silahkan generate sesuai kebutuhan anda
	$nomorTransaksiID = '00000001;

	$response = $bca->fundTransfers($token, 
						$amount,
						$nomorakun,
						$nomordestinasi,
						$nomorPO,
						'Testing Saja Ko',
						'Online Saja Ko',
						$nomorTransaksiID);

	echo json_encode($response);
```

### ACCOUNT STATEMENT

Pastikan anda mendapatkan nilai ```TOKEN``` dan ```TOKEN``` tersebut masih berlaku (Tidak Expired).

```php
	// Nilai token yang dihasilkan saat login
	$token = "MvXPqa5bQs5U09Bbn8uejBE79BjI3NNCwXrtMnjdu52heeZmw9oXgB";

	// Nilai akun bank anda
	$nomorakun = '0201245680';
	
	// Tanggal start transaksi anda
	$startdate = '2016-08-29';
	
	// Tanggal akhir transaksi anda
	$enddate = '2016-09-01';

	$response = $bca->getAccountStatement($token, $nomorakun, $startdate, $enddate);

	echo json_encode($response);
```

### FOREIGN EXCHANGE RATE

```php
	//Tipe rate :  bn, e-rate, tt, tc
	$rateType = 'e-rate';

	$mataUang = 'usd';

	$response = $bca->getForexRate($token, $rateType, $mataUang);

	echo json_encode($response);
```

### NEAREST ATM LOCATOR

```php
	$latitude = '-6.1900718';

	$longitude = '106.797190';

	$totalAtmShow = '10';

	$radius = '20';

	$response = $bca->getAtmLocation($token, $latitude, $longitude, $totalAtmShow, $radius);

	echo json_encode($response);
```

### DEPOSIT RATE

Pastikan anda mendapatkan nilai ```TOKEN``` dan ```TOKEN``` tersebut masih berlaku (Tidak Expired).

```php
	// Nilai token yang dihasilkan saat login
	$token = "MvXPqa5bQs5U09Bbn8uejBE79BjI3NNCwXrtMnjdu52heeZmw9oXgB";

	$response       = $bca->getDepositRate($token);

	echo json_encode($response);
```

# TESTING

Untuk melakukan testing lakukan ```command``` berikut ini

```bash
composer run-script test
```

# LICENSE

MIT License

Copyright (c) 2017 odenktools

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
