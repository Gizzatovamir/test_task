# test_task

# ход работы на текущий момент
# Попытка развернуть concept-graphs локально на машине
При попытке развернуть не получалось собрать пакет из репы https://github.com/krrish94/chamferdist, так как после последних коммитов есть проблемы в коде

Для решения проблемы был найден коммит с работающим кодом, который был вставлен в Dockerfile

В самом коде conect-graphs есть недочеты в возращаемых типах данных, которые были поправлены локально

Далее появились проблемы с версиями куды на локальной машине. Попытки настройки версий и драйверов nvidia не принесли никакого результата (Думаю если бы сидел дольше то получилось бы, но не факт, что это потом можно было возпроизвести на другой машине)

Чтобы максимизоровать шанс воспроизвдеения результатов запуска всего окружения было  принято решение создать Докер контейнер, в котором бы запускался (как минимум) baseline concept-graphs

На данный момент появилась проблема с прокидыванием девайсов (в контейнере не видно ни драйверов видекарты, ни самой видеокарты) в контейнер. 

Попытки решения проблемы:
1. Установка nvidia-container-toolkit с nvidia-docker2 и прокидыванием флага --gpus all при рантайме
2. Попытка выставления флагов окружения для прокидывания девайса в контейнер

Все попытки на данный момент безуспешны. Буду пытаться дальше

build

```bash
docker build  -t concept_graphs_baseline -f src/Dockerfile . --no-cache
```

To run and test env in container

with gpu (Doesn't work yet)
```bash
docker run --name baseline -v ./datasets/replica:/dataset/ -v ./datasets/llava-v1.5-7b:/models --gpus all concept_graphs_baseline:latest sleep infinity
```

without gpu
```bash
docker run --name baseline -v ./datasets/replica:/dataset/ -v ./datasets/llava-v1.5-7b:/models concept_graphs_baseline:latest sleep infinity
```

To stop container

```bash
docker stop baseline
```