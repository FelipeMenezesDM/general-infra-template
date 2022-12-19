# Infraestrutura
Criação de infraestrutura para as contas do Google Cloud Platform, com o GitHub Actions.

## Estrutura
```
.
├── environments/
│   ├── dev/
│   │   └── terraform.tfvars
│   ├── hom/
│   │   └── terraform.tfvars
│   └── prod/
│       └── terraform.tfvars
├── main.tf
└── variables.tf
```

## Criação usando módulos gerais
É recomendado criar recursos de infraestrutura utilizando sempre os [Módulos Gerais](https://github.com/FelipeMenezesDM/general-gcp-modules).

Abaixo, o exemplo de como criar um recurso de secret:
```terraform
# My secret
variable "project_name" {
  type    = string
  default = "my-project"
}

variable "secret_value" {
  type    = string
  default = "initial-value"
}

module "my_secret" {
  source        = "github.com/felipemenezesdm/general-gcp-modules//modules/secret?ref=main"
  project_name  = var.project_name
  secret_name   = "my-secret"
  secret_value  = var.secret_value
}
```

## Destruição de recursos
É possível destruir recursos criados no ambiente cloud diretamente pelo workflow. Para ativar a destruição de todos os recursos, altere a flag "enabled", dentro do arquivo `destroy.yml`:
```yaml
destroy:
  enabled: true
  resources: []
```

Você também pode destruir recursos específicos, listando-os no item "resources":
```yaml
destroy:
  enabled: true
  resources: ["my_resource_1", "my_resource_2"]
```

> **Note**
>
> Lembre-se que os recursos serão criados novamente somente quando a flag "enabled" for alterada para "false".


## Links úteis
- [Módulos gerais](https://github.com/FelipeMenezesDM/general-gcp-modules)
- [Documentação de módulos do Terraform](https://developer.hashicorp.com/terraform/language/modules/develop)
- [Template padrão para projetos de infraestrutura](https://github.com/FelipeMenezesDM/general-infra-template)
