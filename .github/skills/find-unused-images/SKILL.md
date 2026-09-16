---
name: find-unused-images
description: 'Lista imagens em src/images cujo nome de arquivo não aparece em nenhum arquivo versionado fora de src/images. Use quando o usuário pedir para listar/encontrar imagens não usadas, órfãs ou sem referência em src/images.'
---

# Encontrar imagens não usadas em src/images

## Quando usar
- Usuário pede para listar imagens não usadas em `src/images`.

## Procedimento
Execute no PowerShell, a partir da raiz do repositório:

```powershell
$ErrorActionPreference='Stop'; $root=(Get-Location).Path; $imgRoot=Join-Path $root 'src/images'; $exts=@('*.png','*.jpg','*.jpeg','*.bmp','*.gif','*.webp','*.ico','*.svg'); $images=Get-ChildItem -Path $imgRoot -Recurse -File -Include $exts; $unused=[System.Collections.Generic.List[string]]::new(); foreach($img in $images){ $name=$img.Name; $rel=$img.FullName.Substring($imgRoot.Length+1).Replace('\\','/'); git grep -I -F -q -- "$name" -- . ":(exclude)src/images/**"; if($LASTEXITCODE -ne 0){ $unused.Add($rel) } }; "TOTAL_IMAGES=$($images.Count)"; "UNUSED_IMAGES=$($unused.Count)"; '---UNUSED_LIST_START---'; $unused | Sort-Object; '---UNUSED_LIST_END---'
```
