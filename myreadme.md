# 启动方式
uvicorn app:app --host 0.0.0.0 --port 9090

# 测试
curl localhost:9090/vectors -H 'Content-Type: application/json' -d '{"text": "你好"}'

# weaviate的docker-compose.yml内容, 替换http://xx.xx.xx.xx:9090为自己的ip
cat docker-compose.yml
---
version: '3.4'
services:
  weaviate:
    command:
    - --host
    - 0.0.0.0
    - --port
    - '8080'
    - --scheme
    - http
    image: semitechnologies/weaviate:1.20.0
    ports:
    - 8080:8080
    restart: on-failure:0
    environment:
      QUERY_DEFAULTS_LIMIT: 25
      AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED: 'true'
      PERSISTENCE_DATA_PATH: '/var/lib/weaviate'
      DEFAULT_VECTORIZER_MODULE: text2vec-transformers
      ENABLE_MODULES: text2vec-transformers
      TRANSFORMERS_INFERENCE_API: http://xx.xx.xx.xx:9090
      CLUSTER_HOSTNAME: 'node1'
...