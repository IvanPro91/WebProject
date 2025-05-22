
**Инструкция по запуску:**

1.  **Созданы шаблоны HTML `contacts.html`, `catalog.html`, `category.html`, `main.html`
2.  **Описание стартового скрипта веб сервера:**

    ```python
    import json
    import os.path
    from http.server import BaseHTTPRequestHandler, HTTPServer
    
    hostName = "localhost" 
    serverPort = 8080 
    
    
    class Server(BaseHTTPRequestHandler):
        base_dir = os.path.dirname(__file__)
    
        urls_path = {
            "GET":
                {
                    "/catalog": base_dir + "/templates/catalog/catalog.html",
                    "/main": base_dir + "/templates/main/main.html",
                    "/category": base_dir + "/templates/category/category.html",
                    "/contacts": base_dir + "/templates/contacts/contacts.html",
                    "default": base_dir + "/templates/contacts/contacts.html",
                },
            "POST": {
                "/add_message": {"message": "success"}
            }
        }
    
        def do_POST(self):
            urls_path = self.urls_path["POST"]
            if self.path in urls_path:
                content_length = int(self.headers['Content-Length'])
                user_data = self.rfile.read(content_length).decode('utf-8')
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
    
                print(user_data)
                self.wfile.write(json.dumps(urls_path[self.path]).encode("utf-8"))
    
        def do_GET(self):
            self.send_response(200)
            self.send_header("Content-type", "text/html")
            self.end_headers()
            urls_path = self.urls_path["GET"]
            if self.path in urls_path:
                template = urls_path[self.path]
            else:
                template = urls_path["default"]
    
            with open(template, encoding="utf-8", mode="r") as f:
                self.wfile.write(bytes(f.read(), "utf-8"))
    
    
    if __name__ == "__main__":
        webServer = HTTPServer((hostName, serverPort), Server)
        print("Server started http://%s:%s" % (hostName, serverPort))
    
        try:
            webServer.serve_forever()
        except KeyboardInterrupt:
            pass
    
        webServer.server_close()
        print("Server stopped.")

    ```

3.  **Запустите сервер:**

    ```bash
    python run.py
    ```

4.  **Откройте веб-браузер и перейдите по адресу `http://127.0.0.1:8000/`**. Вы должны увидеть страницу "Контакты".  Заполните форму и отправьте её. В консоли сервера должны появиться данные, отправленные из формы.

**Пояснения:**

*   `MyHandler` – это класс, обрабатывающий входящие запросы.
*   `do_GET` – обрабатывает GET-запросы. Он открывает файл `contacts.html`, читает его содержимое и отправляет его обратно клиенту.
*   `do_POST` – обрабатывает POST-запросы.
    *   Считывает тело запроса (данные, отправленные формой).
    *   Декодирует данные.
    *   Выводит данные в консоль.
    *   Отправляет подтверждение клиенту.
*   `HTTPServer` запускает HTTP-сервер на указанном порту (в данном случае, 8080).

**Дополнительные замечания:**

*   Этот код предполагает, что данные отправляются в формате `application/x-www-form-urlencoded`. Для работы с JSON-данными потребуется другая логика декодирования.
*   Для более сложных веб-приложений с обработкой статических файлов, маршрутизацией и т.д.  рекомендуется использовать более продвинутые фреймворки, такие как Flask или Django.

Этот проект демонстрирует создание простого веб-сервера на Python без использования внешних фреймворков. Он показывает, как обрабатывать GET и POST-запросы и извлекать данные из POST-запросов.

## Мини-инструкция: Создание клона репозитория с GitHub

Чтобы создать локальную копию (клон) репозитория с GitHub (в вашем случае, `https://github.com/IvanPro91/WebProject.git`), выполните следующие шаги:

1.  **Установите Git (если у вас его еще нет):**

    *   **Windows:** Скачайте и установите Git с официального сайта: [https://git-scm.com/downloads](https://git-scm.com/downloads).  При установке примите настройки по умолчанию или настройте их по своему желанию.
    *   **macOS:**  Обычно Git уже установлен.  Если нет, установите его через Homebrew: `brew install git`.  Или скачайте с [https://git-scm.com/downloads](https://git-scm.com/downloads).
    *   **Linux (Debian/Ubuntu):**  `sudo apt-get install git`
    *   **Linux (Fedora/CentOS/RHEL):** `sudo yum install git` или `sudo dnf install git`

2.  **Откройте терминал (командную строку):**

    *   **Windows:**  Найдите "Командная строка" или "PowerShell" в меню "Пуск".
    *   **macOS:** Откройте приложение "Терминал" (в папке /Applications/Utilities/).
    *   **Linux:** Откройте ваш терминал по умолчанию (например, GNOME Terminal, Konsole и т.д.).

3.  **Перейдите в директорию, где хотите создать клон (необязательно):**

    *   Используйте команду `cd` (change directory).  Например, чтобы перейти в папку "Documents":  `cd Documents`
    *   Если вы не укажете директорию, клон будет создан в текущей директории терминала.

4.  **Выполните команду `git clone` с URL репозитория:**

    ```bash
    git clone https://github.com/IvanPro91/WebProject.git
    ```

    *   Git скачает все файлы из репозитория `https://github.com/IvanPro91/WebProject.git` в новую папку с тем же именем (WebProject) в текущей директории.

5.  **(Необязательно) Если у вас проблемы с правами доступа (редко):** Если при клонировании возникают ошибки, связанные с правами доступа, убедитесь, что у вас есть разрешение на запись в выбранную директорию.

**После клонирования:**

*   У вас появится локальная копия репозитория в папке `WebProject`.
*   Вы можете переходить в эту папку (`cd WebProject`) и работать с файлами: редактировать, добавлять новые, удалять и т.д.
*   Для отправки изменений обратно на GitHub (или на другой удаленный репозиторий), вам понадобится использовать команды `git add`, `git commit` и `git push` (но это уже другая тема).