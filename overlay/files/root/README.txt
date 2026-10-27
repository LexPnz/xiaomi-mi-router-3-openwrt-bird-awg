BIRD 3 + AmneziaWG 3.1 — Xiaomi Mi Router R3

РУССКИЙ

В прошивке уже установлены BIRD 3 и AmneziaWG 3.1.

1. Настройка AmneziaWG

Чтобы интерфейс awg0 работал через AmneziaWG 3.1, необходимо сгенерировать конфигурацию именно для версии 3.1. Например, это можно сделать на сайте:

https://awgconfig.com/

Импортируйте полученную конфигурацию через LuCI либо настройте интерфейс вручную в /etc/config/network.

Перед настройкой BGP убедитесь, что интерфейс awg0 настроен и соединение AmneziaWG установлено.

2. Настройка BGP через ANTIFILTER.NETWORK

В конфигурацию сети уже добавлен маршрут до BGP-сервера ANTIFILTER.NETWORK (45.154.73.71) через интерфейс awg0. Это необходимо, поскольку сам BGP-сервер находится в сети, блокируемой РКН.

Чтобы использовать VPN совместно с BGP ANTIFILTER.NETWORK, откройте /etc/bird.conf и раскомментируйте соответствующий пример конфигурации.

Рекомендуется изменить router id, указав уникальный идентификатор маршрутизатора.

При корректной настройке BGP через VPN будут направляться сети, анонсируемые BGP-соседом. Остальной трафик будет идти по маршруту по умолчанию, если другие правила маршрутизации не задают иное поведение.

Информация и материалы проекта ANTIFILTER.NETWORK:

https://antifilter.download/

Важно: перед включением BGP проверьте параметры соседа, номера автономных систем (AS), доступность сервера и правила маршрутизации.

3. Проверка маршрутизации BGP

Чтобы проверить, через какой интерфейс маршрутизируется трафик до нужного IP-адреса, выполните:

ip route get 1.1.1.1

Если для этого адреса установлен маршрут через VPN, в выводе должен быть указан интерфейс awg0. Если указан другой интерфейс, например wan, трафик направляется через него.

Адрес 1.1.1.1 приведён только в качестве примера. Результат зависит от маршрутов, полученных через BGP, и текущей таблицы маршрутизации.

ENGLISH

BIRD 3 and AmneziaWG 3.1 are preinstalled in this firmware.

1. Configuring AmneziaWG

To use the awg0 interface with AmneziaWG 3.1, you must generate a configuration specifically for version 3.1. For example, you can use:

https://awgconfig.com/

Import the generated configuration through LuCI or configure the interface manually in /etc/config/network.

Before configuring BGP, make sure that awg0 is configured and the AmneziaWG connection is established.

2. Configuring BGP with ANTIFILTER.NETWORK

A route to the ANTIFILTER.NETWORK BGP server (45.154.73.71) via the awg0 interface has already been added to the network configuration. This is necessary because the BGP server itself is located on a network blocked by Russian internet filtering (RKN).

To use the VPN with ANTIFILTER.NETWORK BGP routing, open /etc/bird.conf and uncomment the relevant example configuration.

It is recommended to change router id to a unique identifier for your router.

When BGP is configured correctly, traffic for networks advertised by the BGP peer will be routed through the VPN. Other traffic will use the default route unless other routing rules specify different behavior.

ANTIFILTER.NETWORK project information and resources:

https://antifilter.download/

Important: before enabling BGP, verify the peer settings, autonomous system (AS) numbers, server reachability, and routing rules.

3. Checking BGP routing

To check which route and network interface Linux selects for a destination IP address, run:

ip route get 1.1.1.1

If a VPN route is installed for this destination, the output should show the awg0 interface. If another interface, such as wan, is shown, traffic is routed through that interface instead.

The address 1.1.1.1 is only an example. The result depends on the routes received through BGP and the current routing table.
