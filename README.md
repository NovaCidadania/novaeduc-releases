# NovaEduc — Distribuição Pública de Releases

Repositório oficial de distribuição de artefatos do app **NovaEduc** para múltiplas plataformas.

Gerado automaticamente a partir de
[`NovaCidadania/novaeduc`](https://github.com/NovaCidadania/novaeduc).

---

## Ambientes

| Ambiente | Prefixo de tag | Finalidade |
|---|---|---|
| `hml` | `hml-x.y.z` | Homologação — testes internos e QA |
| `prd` | `vx.y.z` | Produção — distribuição pública |

---

## Estrutura de diretórios

```
novaeduc-releases/
  hml/
    LATEST                    ← tag mais recente (ex: hml-1.2.3)
    1.2.3/
      release-manifest.json
      checksums.txt
      android/
        novaeduc-hml-android-1.2.3.apk
        novaeduc-hml-android-1.2.3.aab
      linux/
        novaeduc-hml-linux-1.2.3.tar.gz
      flatpak/
        novaeduc-hml-flatpak-1.2.3.flatpak
        com.novaeduc.novaeduc_mobile.yml
        packaging/
      source/
        novaeduc-hml-source-1.2.3.tar.gz
  prd/
    LATEST                    ← tag mais recente (ex: v1.2.3)
    1.2.3/
      (mesma estrutura)
```

---

## Como descobrir e baixar a última release

### APK Android — HML (homologação)

```bash
# 1. Descubra a tag mais recente
TAG=$(curl -sL https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/hml/LATEST)
VERSION="${TAG#hml-}"

# 2. Leia o manifesto
curl -sL "https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/hml/${VERSION}/release-manifest.json"

# 3. Baixe o APK
curl -LO "https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/hml/${VERSION}/android/novaeduc-hml-android-${VERSION}.apk"
```

### APK Android — PRD (produção)

```bash
# 1. Descubra a tag mais recente
TAG=$(curl -sL https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/prd/LATEST)
VERSION="${TAG#v}"

# 2. Leia o manifesto
curl -sL "https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/prd/${VERSION}/release-manifest.json"

# 3. Baixe o APK
curl -LO "https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/prd/${VERSION}/android/novaeduc-prd-android-${VERSION}.apk"
```

---

## Artefatos disponíveis por release

| Artefato | Localização | Convenção de nome |
|---|---|---|
| APK Android | `{env}/{versão}/android/` | `novaeduc-{env}-android-{versão}.apk` |
| AAB Android | `{env}/{versão}/android/` | `novaeduc-{env}-android-{versão}.aab` |
| Bundle Linux | `{env}/{versão}/linux/` | `novaeduc-{env}-linux-{versão}.tar.gz` |
| Flatpak | `{env}/{versão}/flatpak/` | `novaeduc-{env}-flatpak-{versão}.flatpak` |
| Source tarball | `{env}/{versão}/source/` | `novaeduc-{env}-source-{versão}.tar.gz` |
| Checksums | `{env}/{versão}/checksums.txt` | — |
| Manifesto | `{env}/{versão}/release-manifest.json` | — |

---

## release-manifest.json

Cada release publica um manifesto JSON com metadados e paths de todos os artefatos.

```json
{
  "version": "1.2.3",
  "tag": "hml-1.2.3",
  "environment": "hml",
  "runtime_environment": "staging",
  "visibility": "prerelease",
  "build_number": 1002031,
  "commit": "abc1234...",
  "created_at": "2026-04-03T12:00:00Z",
  "discovery_api_base_url": "https://api.hml.novaed.com.br",
  "artifacts": {
    "apk":          { "path": "android/novaeduc-hml-android-1.2.3.apk",  "sha256": "..." },
    "aab":          { "path": "android/novaeduc-hml-android-1.2.3.aab",  "sha256": "..." },
    "linux_bundle": { "path": "linux/novaeduc-hml-linux-1.2.3.tar.gz",   "sha256": "..." },
    "flatpak":      { "path": "flatpak/novaeduc-hml-flatpak-1.2.3.flatpak", "sha256": "..." },
    "source":       { "path": "source/novaeduc-hml-source-1.2.3.tar.gz", "sha256": "..." }
  }
}
```

Use `jq` para extrair campos:

```bash
MANIFEST_URL="https://raw.githubusercontent.com/NovaCidadania/novaeduc-releases/master/hml/${VERSION}/release-manifest.json"

# Tag exata do release
curl -sL "$MANIFEST_URL" | jq -r '.tag'

# Todos os artefatos com SHA-256
curl -sL "$MANIFEST_URL" | jq '.artifacts'

# Caminho do APK
curl -sL "$MANIFEST_URL" | jq -r '.artifacts.apk.path'
```

---

## Verificação de integridade

```bash
cd hml/${VERSION}/
sha256sum --check checksums.txt
```

---

## Instalação Android

1. Habilite "Instalar apps de fontes desconhecidas" nas configurações do dispositivo.
2. Baixe o `.apk` usando os comandos acima.
3. Abra o arquivo no dispositivo para instalar.

---

## GitHub Releases

A aba [Releases](../../releases) também lista todos os artefatos com release notes por tag.
