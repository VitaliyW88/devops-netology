1. Работа c HTTP через телнет.
	Подключитесь утилитой телнет к сайту stackoverflow.com telnet stackoverflow.com 80
	Отправьте HTTP запрос
	GET /questions HTTP/1.0
	HOST: stackoverflow.com
	[press enter]
	[press enter]
В ответе укажите полученный HTTP код, что он означает?

Ответ:

В ответ пришел ответ 403: Сервер понял запрос, но отказывается его выполнять. Мой динамический IP-адрес, предоставленный провайдером, (85.174.207.180) был заблокирован для доступа к сервисам. Если вы считаете, что это ошибка, пожалуйста, свяжитесь с нами по адресу team@stackexchange.com.

Ссылка на скриншот:

https://imgur.com/a/OiLHXFj


HTTP/1.1 403 Forbidden
Connection: close
Content-Length: 1923
Server: Varnish
Retry-After: 0
Content-Type: text/html
Accept-Ranges: bytes
Date: Mon, 06 Feb 2023 22:52:07 GMT
Via: 1.1 varnish
X-Served-By: cache-fra-eddf8230082-FRA
X-Cache: MISS
X-Cache-Hits: 0
X-Timer: S1675723927.055562,VS0,VE1
X-DNS-Prefetch-Control: off

<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Forbidden - Stack Exchange</title>
    <style type="text/css">
                body
                {
                        color: #333;
                        font-family: 'Helvetica Neue', Arial, sans-serif;
                        font-size: 14px;
                        background: #fff url('img/bg-noise.png') repeat left top;
                        line-height: 1.4;
                }
                h1
                {
                        font-size: 170%;
                        line-height: 34px;
                        font-weight: normal;
                }
                a { color: #366fb3; }
                a:visited { color: #12457c; }
                .wrapper {
                        width:960px;
                        margin: 100px auto;
                        text-align:left;
                }
                .msg {
                        float: left;
                        width: 700px;
                        padding-top: 18px;
                        margin-left: 18px;
                }
    </style>
</head>
<body>
    <div class="wrapper">
                <div style="float: left;">
                        <img src="https://cdn.sstatic.net/stackexchange/img/apple-touch-icon.png" alt="Stack Exchange" />
                </div>
                <div class="msg">
                        <h1>Access Denied</h1>
                        <p>This IP address (85.174.207.180) has been blocked from access to our services. If you believe this to be in error, please contact us at <a href="mailto:team@stackexchange.com?Subject=Blocked%2085.174.207.180%20(Request%20ID%3A%201985642390-FRA)">team@stackexchange.com</a>.</p>
                        <p>When contacting us, please include the following information in the email:</p>
                        <p>Method: block</p>
                        <p>XID: 1985642390-FRA</p>
                        <p>IP: 85.174.207.180</p>
                        <p>X-Forwarded-For: </p>
                        <p>User-Agent: </p>

                        <p>Time: Mon, 06 Feb 2023 22:52:07 GMT</p>
                        <p>URL: stackoverflow.com/questions</p>
                        <p>Browser Location: <span id="jslocation">(not loaded)</span></p>
                </div>
        </div>
        <script>document.getElementById('jslocation').innerHTML = window.location.href;</script>
</body>
</html>Connection closed by foreign host.

2. Повторите задание 1 в браузере, используя консоль разработчика F12.
	откройте вкладку Network
	отправьте запрос http://stackoverflow.com
	найдите первый ответ HTTP сервера, откройте вкладку Headers
	укажите в ответе полученный HTTP код
	проверьте время загрузки страницы, какой запрос обрабатывался дольше всего?
	приложите скриншот консоли браузера в ответ.

Ответ:

307, редиект на другой URL, он указан в поле location. В даном случае, адрес тот же, но протокол HTTPS.

Status Code: 307 Internal Redirect

Ссылка на скриншот:

https://imgur.com/a/Lrepncr

Страница полностью загрузилась за 1.25 сек. Самый долгий запрос - начальная загрузка страницы 329 мс

Ссылка на скриншот:

https://imgur.com/a/6OtJBzK

https://imgur.com/a/jxufEbJ

3. Какой IP адрес у вас в интернете?

Ответ:

dig @resolver3.opendns.com myip.opendns.com +short

85.174.207.180

Ссылка на скриншот:

https://imgur.com/a/tyCEmcF

4. Какому провайдеру принадлежит ваш IP адрес? Какой автономной системе AS? Воспользуйтесь утилитой whois

Ответ:

Адрес пренадлежит оператору "Rostelecom Macroregional Branch South"

whois 85.174.207.180 | grep ^descr

descr:          OJSC Rostelecom Macroregional Branch South

Номер AS: AS12389

whois 85.174.207.180 | grep ^origin
origin:         AS12389

Ссылка на скриншот:

https://imgur.com/a/wSWGhjY

5. Через какие сети проходит пакет, отправленный с вашего компьютера на адрес 8.8.8.8? Через какие AS? Воспользуйтесь утилитой 
	traceroute

Ответ:

traceroute -An 8.8.8.8

Пакет проходит AS: AS12389, AS15169.

AS принадлежат моему провайдеру, Ростелеком и Google.

grep org-name <(whois AS12389)

org-name:       PJSC Rostelecom

grep OrgName <(whois AS15169)

OrgName:        Google LLC

Ссылка на скриншот:

https://imgur.com/a/Ofy6DDl

6. Повторите задание 5 в утилите mtr. На каком участке наибольшая задержка - delay?

Ответ:

Дольше всех отвечает 9-ый хоп: AS15169  108.170.250.83

mtr 8.8.8.8 -znrc 1

Start: 2023-02-07T05:05:10+0300
HOST: DESKTOP-2RDS7I5             Loss%   Snt   Last   Avg  Best  Wrst StDev
  1. AS???    172.24.128.1         0.0%     1    0.9   0.9   0.9   0.9   0.0
  2. AS???    192.168.0.1          0.0%     1    3.7   3.7   3.7   3.7   0.0
  3. AS???    100.96.0.1           0.0%     1   11.8  11.8  11.8  11.8   0.0
  4. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
  5. AS12389  94.233.252.170       0.0%     1   11.6  11.6  11.6  11.6   0.0
  6. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
  7. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
  8. AS15169  216.239.49.17        0.0%     1   38.0  38.0  38.0  38.0   0.0
  9. AS15169  108.170.250.83       0.0%     1   74.0  74.0  74.0  74.0   0.0
 10. AS15169  72.14.234.54         0.0%     1   65.0  65.0  65.0  65.0   0.0
 11. AS15169  72.14.232.86         0.0%     1   59.0  59.0  59.0  59.0   0.0
 12. AS15169  209.85.254.135       0.0%     1   57.1  57.1  57.1  57.1   0.0
 13. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 14. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 15. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 16. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 17. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 18. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 19. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 20. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 21. AS???    ???                 100.0     1    0.0   0.0   0.0   0.0   0.0
 22. AS15169  8.8.8.8              0.0%     1   53.9  53.9  53.9  53.9   0.0

Ссылка на скриншот:

https://imgur.com/a/4jfhO2O

7. Какие DNS сервера отвечают за доменное имя dns.google? Какие A записи? Воспользуйтесь утилитой dig

Ответ:

DNS

dig +short NS dns.google

ns3.zdns.google.
ns1.zdns.google.
ns2.zdns.google.
ns4.zdns.google.

A

dig +short A dns.google

8.8.4.4
8.8.8.8

Ссылка на скриншот:

https://imgur.com/a/5Yblwoy

8. Проверьте PTR записи для IP адресов из задания 7. Какое доменное имя привязано к IP? Воспользуйтесь утилитой dig

Ответ:

dns.google

for ip in `dig +short A dns.google`; do dig -x $ip | grep ^[0-9].*in-addr; done

4.4.8.8.in-addr.arpa.   0       IN      PTR     dns.google.

8.8.8.8.in-addr.arpa.   0       IN      PTR     dns.google.

Ссылка на скриншот:

https://imgur.com/a/n5Qadfd
