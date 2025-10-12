# Практично-лабораторне заняття №8
## Тема Неперервна интеграція
## Мета Ознайомитися з принципами і практиками неперервної інтеграції, сформувати навички автоматизації CI/CD процесів в GitHub Actions

### Завдання

1. Пройти цей гайд [Hello GitHub Actions](https://github.com/skills/hello-github-actions?tab=readme-ov-file)

[Посилання на мою копію репозиторія](https://github.com/nick319933/skills-hello-github-actions)

2. [Publish Package](https://github.com/skills/publish-packages)

[Посилання на мій форк Publish Package](https://github.com/nick319933/skills-publish-packages)

2. Створити власний GitHub Workflow для збірки докер-образу front-end
репозиторію та завантаження його у GitHub container registry

2.1. Створити воркфлоу з двома тригерами - ДОКУМЕНТАЦІЯ
• тригер за ручним вкиликом
• тригер за пушем коміту в наступні гілки:
- main
- feature/*

2.2. Воркфлоу має містити 1 job з наступними кроками всередині:

• Клонування репозиторію з налаштуваннями за замовчуванням(див.
actions/checkout)

• Встановлення pnpm за допомогою npm install, встановлення залежностей за
допомогою pnpm install, та збірку проєкту за допомогою pnpm run build.

o Зазначені команди цього пункту мають бути виконані в одному скрипті

• Авторизацію в GitHub Container Registry(див docker/login-action)

o вказати ghcr.io. в полі registry

o вказати власне ім‘я користувача в полі username(

▪ додатково: використайте значення з github context -
ДОКУМЕНТАЦІЯ

o вказати вбудований GITHUB_TOKEN в якості паролю -
ДОКУМЕНТАЦІЯ

• Збірку та завантаження Докер-образу в GitHub container registry(див
docker/build-push-action)

o Вказати поточну директорію в полі context

o Встановити в полі push значення true

o Встановити tag наступного вигляду:

ghcr.io/{{ваш_логін}}/{{ім‘я_репозиторію}}

▪ Логін та ім‘я репозиторію отримати з github context -
ДОКУМЕНТАЦІЯ

2.3. Перейти в свій акаунт GitHub на вкладку Packages та пересвідчитися що там
відображено Ваш Докер-образ

docker-build.yml

name: Build and Push Docker Image

on:
  push:
    branches: [ "main" ]

permissions:
  contents: read
  packages: write  
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v6
        with:
          push: true
          tags: ghcr.io/${{ github.actor }}/vite-react-boilerplate:latest


Перевіряю щоб Actions був успішний
<img width="677" height="325" alt="image" src="https://github.com/user-attachments/assets/a6d090e5-9b51-489d-9f7a-c6e867ee443f" />

Перевіряю що в packages з'явився docker-image

<img width="964" height="113" alt="image" src="https://github.com/user-attachments/assets/b66c9136-aab9-4623-9ad6-bfd859ae698c" />
