# jalali-shamsi-holidays-11-years

روزهای تعطیل رسمی متناسب با تقویم جمهوری اسلامی ایران تا سال 1411 در فایل بالا علامت گذاری شده است

برای جمع آوری این فایل از وب سرویس زیر:

`https://pholiday.herokuapp.com`


با کدهای زیر کراول انجام شد.

``` html 


use Morilog\Jalali\Jalalian;

 public function createNewYearVisits(){
        set_time_limit(0);
        $startDate = Jalalian::fromFormat('Y-m-d', '1400-11-22');
        $to = Jalalian::fromFormat('Y-m-d', '1411-01-01');

        for ($i = $startDate; $i < $to;$i=$i->addDays(1) ) {
            $date = $i->format('Y-m-d');
            $json = json_decode(geturl("https://pholiday.herokuapp.com/date/$date/holiday"));
            while(!isset($json->isHoliday)){
                sleep(1);
                $json = json_decode(geturl("https://pholiday.herokuapp.com/date/$date/holiday"));
            }
            $txt = $date.' '.$json->isHoliday;
            $myfile = file_put_contents('JalaliHolidays1.txt', $txt.PHP_EOL , FILE_APPEND | LOCK_EX);
        }
}	


function geturl($url){
    $curl = curl_init();

    curl_setopt_array($curl, array(
        CURLOPT_URL => $url,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => '',
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 0,
        CURLOPT_FOLLOWLOCATION => true,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => 'GET',
        CURLOPT_HTTPHEADER => array(
            'User-Agent: Mozilla/5.0 (Windows NT 6.2; Win64; x64; rv:85.0) Gecko/20100101 Firefox/85.0',
            'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8',
            'Accept-Language: en-US,en;q=0.5',
            'Connection: keep-alive',
            'Upgrade-Insecure-Requests: 1',
            'Cache-Control: max-age=0'
        ),
    ));

    $response = curl_exec($curl);

    curl_close($curl);
    return $response;
}
```	
		
