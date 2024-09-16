# tekton-hadolint

**Tekton-Hadolint** é uma solução para validar Dockerfiles com o [Hadolint](https://github.com/hadolint/hadolint), um linter de Dockerfile que ajuda a garantir boas práticas no uso de imagens Docker. Este repositório integra o Hadolint com Tekton Pipelines para automatizar a verificação de Dockerfiles em um pipeline CI/CD.

## Referências:
- GitHub: [Hadolint](https://github.com/hadolint/hadolint)
- TekTon Hub: [Hadolint Task](https://hub.tekton.dev/tekton/task/hadolint)

## Estrutura de Diretórios:
```
crds/
├── hadolint.yaml        # Definição do ClusterTask com o Hadolint parametrizado
├── pipelinerun.yaml     # Definição do PipelineRun para executar o Pipeline
├── pipeline.yaml        # Definição do Pipeline Tekton para executar o Hadolint
├── pvc.yaml             # Definição do Persistent Volume Claim (PVC) necessário para o workspace
└── kustomization.yaml   # Arquivo Kustomization para orquestrar todos os YAMLs acima
```

## Executando com OpenShift:

Para aplicar todos os recursos (Pipeline, PipelineRun, PVC, etc.) de uma vez no OpenShift, utilize o seguinte comando:

```bash
oc apply -k crds/
```

Este comando irá aplicar os recursos definidos no diretório `crds/` na ordem especificada no arquivo `kustomization.yaml`.

## Executando com kubectl:

Se você estiver utilizando **kubectl** (por exemplo, em um cluster Kubernetes), use o comando:

```bash
kubectl apply -k crds/
```

Assim como no OpenShift, este comando aplicará todos os recursos em sequência, garantindo que o PVC seja criado antes do Pipeline e do PipelineRun.

## Detalhes dos Recursos:

- **PVC (`pvc.yaml`)**: Define o Persistent Volume Claim necessário para armazenar os dados durante a execução do pipeline.
- **Pipeline (`pipeline.yaml`)**: Define o pipeline Tekton, que inclui a task do Hadolint e a tarefa de clonar o repositório.
- **PipelineRun (`pipelinerun.yaml`)**: Define a execução do pipeline para verificar Dockerfiles.
- **Hadolint Task (`hadolint.yaml`)**: (Opcional) Configurações específicas do Hadolint, se aplicável, ou ajustes na task para personalização.
