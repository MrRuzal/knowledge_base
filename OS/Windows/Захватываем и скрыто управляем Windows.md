#Windows #Windows10
**Удаленное управление Windows (WinRM) –** это применение корпорацией Майкрософт протокола WS-Management, простого доступа к объектам (SOAP), работа которого связана с брандмауэром. Протокол позволяет взаимодействовать оборудованию и операционным системам разных производителей.

Особенности протокола WS-Management предоставляют общий доступ к системе, ее управлению и обмену информацией в рамках IT-инфраструктуры. WinRM, интеллектуальный интерфейс управления платформой (IPMI) и Event Collector являются компонентами оборудования для осуществления удаленного управления Windows.

При сканировании ОС Windows пользователь имеет возможность наткнуться на данный процесс. На скрине изображены порты номер 5985 (http) и 5986 (https).

![](https://sun9-8.userapi.com/s/v1/ig2/eCOnBY33HEdGihQzy74YIm2iDpoXtdU6zzqoXNC_qbsMm-CLhtOoMRLkvEUx-o8qdbClotk7hTfDxhVoUGtT808S.jpg?size=602x212&quality=96&type=album)

По умолчанию служба не работает, и даже если она включена (как в случае с Windows 2008), то порт не прослушивается, а значит трафик не передается. Таким образом, подобный процесс очень трудно обнаружить, но, все-таки, он осуществляется.

Некоторым системам необходима возможность дистанционного выполнения, поэтому пользователям придется активировать службу. Однако они могут и не знать, что это потенциальная угроза. Другая проблема заключается в том, что продавцы либерально относятся к гарантированной поддержке системных функций. Конечно, такой подход работает только на благо покупателей, но должен применяться в меру.

Как можно заметить по поиску Shodan, в интернете есть несколько систем, с которыми работает служба WinRM. Они доступны постоянно и находятся под доменом «Workgroup». Атака может быть выполнена с легкостью, поскольку существуют определенные ограничения доверенного домена.

![](https://sun9-40.userapi.com/s/v1/ig2/EjXV1GuK0FEd7D5vkoW7VCpMYxkddbl9nU3ktKvGYaaBQ77WMSSZwTAIHbi1IFSSsmWsnAjuxrPAocGMEF2MtOj9.jpg?size=602x366&quality=96&type=album)

Существует несколько скриптов, доступных для связи с этой службой. Однако наиболее распространенным методом считается использование модулей Metasploit.

Если есть желание использовать Ruby, об этом подробнее можно прочитать [здесь](https://github.com/WinRb/WinRM).

**gem install-r winrm**

Metasploit предварительно устанавливается на такие ОС, как Kali или Parrot, поэтому чтобы начать, стоит всего лишь выполнить команду: **msfconsole**

![](https://sun9-16.userapi.com/s/v1/ig2/1GWDCq2S1QqSstnBamv957R76_8DWxQ2RgkLCHStM1Q3RUrxDnTmU9t-hJR0_mpQPXgGVzfweYR7mjcrzxDe7B8m.jpg?size=602x375&quality=96&type=album)

Сначала нужно просканировать систему, чтобы проверить, запущен ли процесс. Для этого используется сканер _Winrm_auth_methods_, он проверяет, работает ли служба.

Используется следующая комбинация: _auxiliary/scanner/winrm/winrm_auth_methods_

Пользователю нужно будет ввести соответствующие параметры, но сначала ему стоит немного узнать о характеристиках оборудования. Например, если порт показывает 5986, следует использовать SSL. Очень трудно определить дистанционно конфигурацию сервиса, поэтому иногда применяется метод проб и ошибок.

![](https://sun9-10.userapi.com/s/v1/ig2/ARHyxiU3yfkx9cNibp5jxGSw0UmjTyj6sPiie_adtM5sN-CJGzNxQebAsa_3Fapov_yRatzHswNu-90IgcZC4FiA.jpg?size=602x295&quality=96&type=album)

Как только есть уверенность в том, что цель уязвима, можно атаковать.

Используется следующая комбинация _auxiliary/scanner/winrm/winrm_login_

Опять же, нужно будет ввести необходимые параметры.

![](https://sun9-70.userapi.com/s/v1/ig2/qR2NrbTcwTxGyjWRm40yh532dO8mPJty3Z0YxS9CkG87Lis4udKTsBKvRmpOvBWDJ464yULs9w_XEbY60KTN6fCS.jpg?size=602x348&quality=96&type=album)

Очень важно правильно определить домен, так как для этой службы существует требование доверенного домена (по умолчанию). Его можно заполучить путем сканирования дополнительных портов. Если компьютер входит в Workgroup, у пользователя не должно быть никаких проблем.

![](https://sun9-1.userapi.com/s/v1/ig2/J6EGZzdjQI2M83JbL8RKqLDCHmP7ESj4MA3Ym_VHWVzdmN-W4Q2AkPxPE0Dsvro3Jd0z7Lamw_yTgjx9iCAkgtQs.jpg?size=602x210&quality=96&type=album)

На этом скрине видно, что учётные данные получены.

Теперь есть возможность взаимодействовать с удаленным кодом в системе. Будучи администратором, пользователь способен совершить много «разрушительных» действий. Он также станет доверенным лицом, ведь обладает необходимыми данными. Вредоносный код точно нанесет удар злоумышленникам, обезопасив его создателя. Таким образом, человек способен обойти антивирусную защиту.

![](https://sun9-73.userapi.com/s/v1/ig2/f6uEnsZCrHtmoMaDZfukG4PlWgx-H_Z2kuoBd0cEKVPkeESVICGgm7VxWqVF6UJApMzqCKERh3qcAPI92h7VKjUw.jpg?size=602x243&quality=96&type=album)

Несколько строк кода могут помочь пользователю получить учетные данные. Не обязательно иметь Linux или Metasploit, для того чтобы проникнуть в чужую систему. Поскольку в ваш Windows встроен сервис PowerShell, с помощью которого можно следующие команды:

**Командная строка**:

Winrs — это команда, которая может быть использована для выполнения удаленных команд. В приведенном ниже примере показано, как передаются учетные данные и запускается «whoami»

![](https://sun9-51.userapi.com/s/v1/ig2/271DRjaYN03znmsh4UVdtRsF_la304eYKbP8p5ui99R4kM4RZgTqEb6AYqfn92gJpxzl4UKpDyzIcWdT2nOh0846.jpg?size=602x42&quality=96&type=album)

**PowerShell:**

Также можно использовать несколько команд Powershell для сканирования или выполнения удаленного кода посредством клиента службы WinRM/WsMan.

Тest-Wsman:

![](https://sun9-47.userapi.com/s/v1/ig2/qvZfl0pCPKXktg2XeGgggrd3YhuMZWPTJpTmybGNNAjdgRqpMU4GpaiYHJaTIOemjAWzue7gy_7QG_Up4fpgL6k8.jpg?size=602x175&quality=96&type=album)

Test-NetConnection:

![](https://sun9-77.userapi.com/s/v1/ig2/F6_4s8DCCiHyhI6nSIQRmmnfMuGhAoIAWsO7aEGAF0CC1Uu2lCpEYMJJ_vViCMWUICtZfp14MJTdDW0etvm3eO8-.jpg?size=602x114&quality=96&type=album)

Invoke-Command

![](https://sun9-59.userapi.com/s/v1/ig2/t8f1MLREe-Sel3Y2_mHAU1fNBgQJ35bPnpEUbcpdymTsseDJE9Dt72fiIe6SjW9ryRewyqdYFd8kQ39YC5Mvebtk.jpg?size=602x275&quality=96&type=album)

Однако это очень утомительный метод. Именно поэтому был создан скрипт, который позволяет сканировать и атаковать WinRM с помощью системы Windows.

Подробнее об этом [тут](https://github.com/ctrlaltdel-blog/WinRM_Brute_Scanner).

Рекомендуется не запускать сервисы по умолчанию. Если они все же включены, стоит прочитать [следующую документацию](https://docs.microsoft.com/en-us/windows/win32/winrm/installation-and-configuration-for-windows-remote-management) и активировать некоторые ограничения.

Эта полезная [статья](https://devblogs.microsoft.com/powershell/compromising-yourself-with-winrms-allowunencrypted-true/) рассказывает об ином способе получения учетных данных. Он был придуман еще в 2015 году, но до сегодняшнего дня считается действенным.

WinRM может предоставить постоянный доступ к компьютеру. Ниже приведены команды, которые можно выполнить для включения службы на необходимой машине.

Сервис запускается автоматически по умолчанию, так что даже если система будет перезагружена, он продолжит свою работу.

Следующие команды должны быть запущены от имени администратора:

_winrm quickconfig_

_winrm set winrm/config/Client @{AllowUnencrypted = “true”}_

_Set-Item WSMan:localhostclienttrustedhosts —value “*”_

Преимущество данного способа заключается в том, что это встроенная функция Windows, поэтому никакие антивирусы не должны реагировать на подобную активность.

Если есть желание защититься от подобных атак, стоит принудительно шифровать и ограничивать доверенные хосты. Требуется всего лишь заменить «*» на «server1, server2».