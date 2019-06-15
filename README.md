# swarm-stack
docker swarm stack deploy demo

```
docker swarm init --advertise-addr <local ip>
```
```
ssh user@<VM-Machine1>

docker swarm join --token XXXXXX-X-3XXXXXXXXXXXXXXXXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXX 192.168.1.8:2377

ssh user@<VM-Machine2>
docker swarm join --token XXXXXX-X-3XXXXXXXXXXXXXXXXXXXXXXXXXX-XXXXXXXXXXXXXXXXXXXXXXX 192.168.1.8:2377
exit
```
```
eric@alienware:~/projects/src/github.com/docker/traefik-swarm/ghost-stack$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
23dfq1l3i3ku2vw3huu07y3u9 *   alienware           Ready               Active              Leader              0.0.0-20190612010257-8feff36
x4t00cdi7lbepnllvp88lplyj     swarm               Ready               Active                                  18.09.6
qgngrj1wdxru4xdlpj4zsvwmx     swarm1              Ready               Active                                  18.09.6
```
```
docker node inspect --format '{{.Status.Addr}}' swarm
192.168.122.175
```
```
docker stack deploy --compose-file=portainer-agent-stack.yml portainer
docker service ls
docker service ls
ID                  NAME                  MODE                REPLICAS            IMAGE                        PORTS
r56tvhvahdqv        portainer_agent       global              3/3                 portainer/agent:latest       
nf0u42mf87sw        portainer_portainer   replicated          1/1                 portainer/portainer:latest   *:9000->9000/tcp

```
## Portainer UI
You should be able to access your portainer UI now on one of your agents IP:9000

```
docker service ps portainer_agent
ID                  NAME                                        IMAGE                    NODE                DESIRED STATE       CURRENT STATE         ERROR               PORTS
1cpxc6kqg956        portainer_agent.m7tm3y98ytoyt0llq67qtvwuo   portainer/agent:latest   swarm               Running             Running 2 hours ago                       
20qmlfbfzl33        portainer_agent.qgngrj1wdxru4xdlpj4zsvwmx   portainer/agent:latest   swarm1              Running             Running 2 hours ago                       
```
## Tick Stack
This next part I got the conf and compose files from [https://github.com/mlabouardy/swarm-tick](https://github.com/mlabouardy/swarm-tick)
I also created 1 more manager and 1 more agent for fault [tolerance](https://docs.docker.com/engine/swarm/admin_guide/)
```
docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
23dfq1l3i3ku2vw3huu07y3u9 *   alienware           Ready               Active              Leader              0.0.0-20190612010257-8feff36
x4t00cdi7lbepnllvp88lplyj     swarm               Ready               Active                                  18.09.6
qgngrj1wdxru4xdlpj4zsvwmx     swarm1              Ready               Active                                  18.09.6
m7tm3y98ytoyt0llq67qtvwuo     swarm2              Ready               Active              Reachable           18.09.6
if3cady0iv0mxdwk1b7vtqvxz     swarm3              Ready               Active                                  18.09.6
```
![Portainer Swarm Fault tolerance](./assets/images/portainerui.png)

