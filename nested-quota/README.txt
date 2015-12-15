=====================
Nested Quotas patches
=====================

Esta pasta contém a seguinte estrutura de pastas:
- cinder
- nova

Nas subpastas cinder e nova, nós temos os patches organizados de acordo com a
implementação de nested quotas no Cinder e Nova, respectivamente.

Visão Geral
~~~~~~~~~~~

A funcionalidade Nested Quotas foi divida em vários patches tanto no Cinder
quanto no Nova com o objetivo de facilitar o processo de revisão e
implementação. A seguir apresentamos um resumo do objetivo de cada patch:

#. cinder/cinder_fixes_bug_default_subprojects.patch

Este patch corrige o valor default de cotas para subprojetos.
Ao realizar uma operação ``default`` para as cotas de um projeto,
o DBQuotaDriver checa apenas se a configuração ``default_quota_class``
está setada para True. Mas esse valor de configuração não deveria ser
considerado ao requisitar as cotas default para subprojetos. Sem as
modificações deste patch, ao requisitar as cotas defaults de um subprojeto,
os valores retornados eram os mesmos de um projeto raiz.

#. cinder/cinder_fixes_bug_delete_quota_subprojects.patch

Este patch corrige o bug em que o cinder lança uma exceção ao realizar uma
operação de ``delete`` cota em um subprojeto quando o projeto pai tem cotas
default.

#. cinder/cinder_fixes_bug_update_quota_subprojects.patch

Este patch corrige o bug em que o cinder lança uma exceção ao realizar uma
operação de ``update`` cota em um subprojeto quando o projeto pai tem cotas
default. Como solução proposta, adicionamos as cotas default como entradas
no banco de dados ao realizar uma operação de ``update`` cota em um
subprojeto e não há nenhuma cota explícitamente associada ao projeto pai.

#. cinder/cinder_nested_quota_driver.patch

Neste patch são realizados todos os cálculos relativos à cota disponível
para subprojetos, além de verificar permissões de acesso para realizar as
operações em uma hierárquia de projetos (No Cinder).

#. cinder/cinder_nested_quota_get_project_hierarchy.patch

Neste patch implementamos a comunicação entre o Cinder e o Keystone, de
forma a recuperar as informações de um projeto e saber a qual hierarquia
ele pertence e se é um subprojeto ou um projeto raiz (No Cinder).

#. cinder/cinder_nested_quota_set_default_to_subprojects.patch

Neste patch modificamos as cotas defaults para projetos que não são raízes
em uma hierárquia. A cota default para subprojetos, passa a ser 0, de forma
que a cota do projeto pai não seja totalmente consumida ao criar um
subprojeto (No Cinder).

#. cinder/openstack-manuals_add_documentation_nested_quotas.patch

Este patch adiciona documentação sobre a funcionalidade Nested Quotas no
Cinder, com o objetivo de facilitar o entendimento de usuários que desejem
utilizar Nested Quotas no Cinder. Obs: este patch foi submetido para o
repositório openstack-manuals.

#. nova/nova_nested_quota_create_column_allocated.patch

Neste patch, adicionamos a coluna ``allocated`` na tabela quotas, de forma
a manter controle sobre quanto da cota de um projeto foi forneciada para
subprojetos em sua hierárquia.

#. nova/nova_nested_quota_driver.patch

Neste patch são realizados todos os cálculos relativos à cota disponível
para subprojetos, além de verificar permissões de acesso para realizar as
operações em uma hierárquia de projetos (No Nova).

#. nova/nova_nested_quota_get_project_hierarchy.patch

Neste patch implementamos a comunicação entre o Cinder e o Keystone, de
forma a recuperar as informações de um projeto e saber a qual hierarquia
ele pertence e se é um subprojeto ou um projeto raiz (No Nova).

#. nova/nova_nested_quota_set_default_subprojects.patch

Neste patch modificamos as cotas defaults para projetos que não são raízes
em uma hierárquia. A cota default para subprojetos, passa a ser 0, de forma
que a cota do projeto pai não seja totalmente consumida ao criar um
subprojeto (No Nova).
