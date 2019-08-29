# PHP Runner

These docker images are meant for testing, building & deploying PHP code from CI tools that support docker images.  
You shouldn't use the images to run production servers.

## Versions

### PHP 7.2
`atymic/php72-runner`
![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/atymic/php72-runner)
![Docker Pulls](https://img.shields.io/docker/pulls/atymic/php72-runner?style=flat-square)

### PHP 7.3
`atymic/php73-runner`
![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/atymic/php73-runner)
![Docker Pulls](https://img.shields.io/docker/pulls/atymic/php73-runner?style=flat-square)

## Using the image

### Gitlab
This is an example Gitlab pipline. You'll probably need to customise it for your purposes

```yaml
stages:
  - build
  - deploy

build:php:
  image: atymic/php73-runner
  stage: build
  script:
    - phpcs
    - phan # AST extension is installed
    - ./vendor/bin/phpunit # or use local version

deploy:
  stage: deploy
  image: atymic/php73-runner
  before_script:
    - ssh-add <(echo "$PRIVATE_KEY")
  script:
    - dep deploy production --tag=$CI_COMMIT_REF_NAME -vvv
  only:
    - master
```

## Features
- Pre-built with almost all extensions
- Use composer github token to avoid rate limits (`$COMPOSER_GITHUB`)
- Set container time zone (`$TIMEZONE`)
- Optional Xdebug support (`WITH_XDEBUG=true`)
- Node, NPM & Yarn installed (but you should use a dedicated image for building JS/CSS if possible)
- Common PHP build, linting, analysis and deployment tools baked in:
    - `phpunit`
    - `phpmd`
    - `php_codesniffer`
    - `phpstan`
    - `psalm`
    - `phan`
    - `deployer` (& recipes)
- Deployment tools (`rsync`, `ssh-agent`, `git`) 


## Credits

These images are based on [ambroisemaupate's work.](https://github.com/ambroisemaupate/docker/)
