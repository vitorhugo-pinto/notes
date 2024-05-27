Containers são processos executados.

- namespaces → isola os processos
- cgroups → controla os recursos do container
- file system → permite gerenciar os arquivos dentro do container quando este está rodando.

Dockerfile: contém instruções e configurações para realizar o _build_ de uma imagem.

- Imagem esta que terá estado imutável, com uma camada capaz de receber função de leitura e escrita (read/write).

**Estrutura Docker**

- _Docker client_: se comunica com o _Docker Host,_ através de _CLI_, o client passa comandos para o _daemon API_ do _Docker Host;_
    - Roda o container;
    - Ações de run, pull e push;
        - run: executa comandos para ativação de container;
        - pull: puxa uma imagem do registry de imagens (_Dockerfile_);
        - push: sobe uma imagem gerada para o registry de imagens;
    - Configura o uso de _Volumes_ e _Network_ do _Docker Host_;
- _Docker Host_: ambiente de execução de aplicações, recebe os comandos do cliente via _CLI_ através do _daemon API_. Gerencia:
    - Volumes: este é capaz de espelhar num diretório local os artefatos gerados dentro de um container, pois o container quando encerra seu processo nenhuma informação dentro dele é persistida sem configurar o manuseio do volume (storage). Relação com a imutabilidade dos containers.
        
    - Network: responsável pela comunicação do container, permite que meios externos se comuniquem com o container.
        
        - **Portas**
            - Porta ativa num processo não quer dizer que seja possível acessar essa porta pelo nosso cliente, essa porta fica disponível para outros processos na mesma rede (network), mas não para o externo.
    - Cache: ligado ao _Registry,_ capaz de armazenar em cache imagens do Registry para facilitar e agilizar montagens de containers (download e upload de imagens) (pull e push).