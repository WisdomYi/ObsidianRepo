## 1.指定固定IPv4
```
1.networks 部分自定义网络，并指定子网和网关

networks:
  my_network:
    driver: bridge
    ipam:
      config:
        - subnet: 172.20.0.0/16  # 子网范围
          gateway: 172.20.0.1    # 网关地址


2.在服务的networks配置中，为该网络指定ipv4_address

services:
  app1:
    image: nginx
    networks:
      my_network:
        ipv4_address: 172.20.0.2  # 指定静态IP
    restart: always
ps:
- ip不能重复
- 网络的驱动通常使用bridge

```