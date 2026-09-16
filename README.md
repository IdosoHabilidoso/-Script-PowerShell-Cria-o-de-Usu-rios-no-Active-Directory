# -Script-PowerShell-Cria-o-de-Usu-rios-no-Active-Directory
Este script cria usuários automaticamente no AD, define senha, coloca em grupos e organiza em OUs. É ótimo para estudos, laboratórios e demonstração de habilidades.
# Script para criar usuários no Active Directory
# Autor: Giuliano Moraes
# Objetivo: Automatizar a criação de usuários em um ambiente AD

Import-Module ActiveDirectory

# Lista de usuários a serem criados
$usuarios = @(
    @{Nome="Joao Silva"; Usuario="joao.silva"; Senha="P@ssw0rd123"; OU="OU=Usuarios,DC=dominio,DC=local"; Grupo="TI"},
    @{Nome="Maria Souza"; Usuario="maria.souza"; Senha="P@ssw0rd123"; OU="OU=Usuarios,DC=dominio,DC=local"; Grupo="Financeiro"},
    @{Nome="Carlos Pereira"; Usuario="carlos.pereira"; Senha="P@ssw0rd123"; OU="OU=Usuarios,DC=dominio,DC=local"; Grupo="RH"}
)

foreach ($u in $usuarios) {

    Write-Host "Criando usuário: $($u.Nome)"

    # Criação do usuário
    New-ADUser `
        -Name $u.Nome `
        -SamAccountName $u.Usuario `
        -UserPrincipalName "$($u.Usuario)@dominio.local" `
        -AccountPassword (ConvertTo-SecureString $u.Senha -AsPlainText -Force) `
        -Enabled $true `
        -Path $u.OU `
        -ChangePasswordAtLogon $false

    # Adiciona ao grupo
    Add-ADGroupMember -Identity $u.Grupo -Members $u.Usuario

    Write-Host "Usuário $($u.Usuario) criado e adicionado ao grupo $($u.Grupo)."
}

Write-Host "Processo concluído!"
