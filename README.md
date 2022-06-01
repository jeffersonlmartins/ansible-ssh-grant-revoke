# ansible-ssh-conceder-revoke
Instruções para Conceder ou Revogar acesso SSH:

### Para adicionar um novo usuário e conceder um acesso SSH:
ansible-playbook -i inv -e "action=conceder" ssh.yml
```
 [WARNING]: Found variable using reserved name: action


PLAY [servers] ******************************************************************************************************************************************************

TASK [ssh : Adicionando novo usuario] ************************************************************************************************************************************
changed: [servidor1]
changed: [servidor2]

TASK [ssh : Concedendo acesso SSH] ***************************************************************************************************************************
changed: [servidor1]
changed: [servidor2]

TASK [ssh : Revogando usuario acesso SSH] ************************************************************************************************************************
skipping: [servidor1]
skipping: [servidor2]

TASK [ssh : Removendo usuario] *********************************************************************************************************************************
skipping: [servidor1]
skipping: [servidor2]

PLAY RECAP **********************************************************************************************************************************************************
servidor1                    : ok=2    changed=2    unreachable=0    failed=0
servidor2                    : ok=2    changed=2    unreachable=0    failed=0

```

### Conceder acesso SSH para um user existente (pulando a etapa de criação de um usuário)
ansible-playbook -i inv -e "action=conceder" ssh.yml --skip-tags=adicionar

### Para revogar o acesso SSH de um usuário existente (pulando a etapa de remoção do usuário)
ansible-playbook -i inv -e "action=revogar" ssh.yml --skip-tags=remover

### Para remover e revogar o usuário
ansible-playbook -i inv -e "action=revogar" ssh.yml
```
 [WARNING]: Found variable using reserved name: action


PLAY [servers] ******************************************************************************************************************************************************

TASK [ssh : Adicionando novo usuario] ************************************************************************************************************************************
skipping: [servidor1]
skipping: [servidor2]

TASK [ssh : Concedendo acesso SSH] ***************************************************************************************************************************
skipping: [servidor1]
skipping: [servidor2]

TASK [ssh : Revogando usuario acesso SSH] ************************************************************************************************************************
changed: [servidor1]
changed: [servidor2]

TASK [ssh : Removendo usuario] *********************************************************************************************************************************
changed: [servidor1]
changed: [servidor2]

PLAY RECAP **********************************************************************************************************************************************************
servidor1                    : ok=2    changed=2    unreachable=0    failed=0
servidor2                    : ok=2    changed=2    unreachable=0    failed=0

```

### Para verificar os resultados, faça login em sua conta "user_name" e ssh em um de seus servidores
```
[dev@ip-192-168-100-15 ~]$ ssh 192.168.100.20
Last login: Wed Jun  1 03:06:34 2022 from ip-192-168-100-15.ec2.internal

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

[dev@ip-192-168-100-15 ~]$ 
```

## Observações
1. Certifique-se de adicionar IPs ou DNS dos servidores de destino no arquivo inv.
2. Atualize as variáveis "user_name" e "user_des" no arquivo inv.
3. Crie um diretório no diretório "keys" com o mesmo nome de "user_name" e coloque sua chave pública SSH nele.
4. Configure um usuário (no meu caso - ec2-user) que deve ter acesso sem senha a ambos os servidores para executar nossos scripts ansible.
