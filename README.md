# Traefik Docker Stack with Mantrae UI & Cloudflare DNS
Этот репозиторий содержит готовую к развертыванию конфигурацию `docker-compose` для запуска обратного прокси-сервера [Traefik](https://traefik.io/) с автоматическим получением wildcard-сертификатов через DNS-челлендж Cloudflare, панелью управления [Mantrae](https://github.com/MizuchiLabs/mantrae) и тестовым сервисом `whoami`.

## Возможности

- **Автоматические SSL-сертификаты**: Wildcard-сертификаты Let's Encrypt через DNS-челлендж Cloudflare.
- **Безопасность по умолчанию**: `no-new-privileges`, отключение небезопасного API и дашборда Traefik напрямую.
- **Готовые сервисы**:
    - **Mantrae**: Современная панель управления для Traefik.
    - **Whoami**: Тестовый сервис для отладки заголовков и сетевой связанности.

## Требования

*   **Docker** и **Docker Compose**.
*   Домен, делегированный на **Cloudflare**.
*   **Cloudflare API Token** с правами на редактирование DNS-зоны (Zone:Zone:Read, Zone:DNS:Edit).

### Уствновка и настройка

```bash
git clone https://github.com/Fierst234/traefik-mantrae.git && cd traefik-mantrae
```
Необходимо заменить стандартные значения в .env.example на свои
```bash
cp .env.example .env
```
```bash
nano .env
```
```bash
docker compose up -d
```

### Доступные сервисы

- **Mantrae** - https://mantrae.yourdomain.com
- **Traefik Dashboard** (выключен) - https://traefik.yourdomain.com/dashboard
- **Whoami** (выключен) - https://whoami.yourdomain.com

Сервисы whoami и нативный дашборд закомментированы в конфигурации. Чтобы их включить, раскомментируйте соответствующие блоки labels в docker-compose.yml.

### Как это работает

1. Traefik слушает порты 80 и 443 на хост-машине.
2. При запуске нового контейнера с меткой traefik.enable=true, Traefik автоматически обнаруживает его через Docker API и создает динамическую конфигурацию.
3. Для каждого маршрута используется резолвер cloudflare, который запрашивает wildcard-сертификат (*.yourdomain.com) через DNS-челлендж.
4. Mantrae предоставляет веб-интерфейс для мониторинга и управления этими маршрутами.
