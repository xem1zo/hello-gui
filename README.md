# hello-gui

Учебный проект с CI/CD на GitHub Actions для **Go GUI (Fyne)** с публикацией бинарников в **GitHub Releases**.

## 📌 Цель работы

Научиться автоматически собирать GUI-бинарники Go под Linux, macOS и Windows и публиковать их в GitHub Releases при push тега `v*`.

## 🎯 Что делает CI/CD

**Job `test`** — запускается при любом push:
- `gofmt -l` — проверка форматирования
- `go vet` — линтер
- `go test -v` — тесты
- `go build` — smoke-сборка

**Job `release`** — запускается **только при push тега `v*`**:
- Матричная сборка под 3 ОС:
  - `ubuntu-latest` → `hello-gui-linux-x64`
  - `macos-14` → `hello-gui-macos-arm64`
  - `windows-latest` → `hello-gui-windows-x64.exe`
- Сборка Windows использует MSYS2 + MinGW-w64 GCC (из-за CGO)
- Публикация файлов в **GitHub Release**

## 📂 Структура проекта

```text
hello-gui/
├── .github/workflows/ci.yml
├── Dockerfile.test
├── logic.go
├── logic_test.go
├── main.go
├── go.mod
├── go.sum
└── README.md
```

## 🎨 Интерфейс приложения


## 🚀 Создание релиза

```bash
git tag v0.1.0
git push origin v0.1.0
```

Workflow автоматически:
1. Прогонит формат-чек, `go vet` и тесты
2. Соберёт 3 бинарника параллельно (Linux, macOS, Windows)
3. Создаст GitHub Release `v0.1.0` с прикреплёнными файлами

## 📥 Скачивание и запуск

### Windows (PowerShell)

```powershell
Invoke-WebRequest `
  -Uri "https://github.com/xem1zo/hello-gui/releases/download/v0.1.0/hello-gui-windows-x64.exe" `
  -OutFile "hello-gui.exe"

.\hello-gui.exe
```

### Linux / macOS

```bash
wget https://github.com/xem1zo/hello-gui/releases/download/v0.1.0/hello-gui-linux-x64 -O hello-gui
chmod +x hello-gui
./hello-gui
```

## 🛠 Технологии

- **Go 1.23**, **Fyne v2** — GUI
- **CGO**, **GLFW**, **OpenGL**, **X11**, **Wayland** — компиляция GUI
- **MSYS2 + MinGW-w64 GCC** — для сборки под Windows
- **GitHub Actions** — CI/CD
- **softprops/action-gh-release** — публикация релизов
- **Docker** — локальная сборка и тесты

## 📝 Вывод

Освоил:
- Fyne — виджеты, контейнеры, окна
- `go mod tidy` — управление зависимостями, `go.sum`
- CGO-зависимости для Linux (X11, Wayland, OpenGL)
- Установку GCC через MSYS2 для сборки под Windows
- Cross-compilation GUI (требует нативные компиляторы → 3 runner'а)
- Флаг `-H windowsgui` — скрыть консоль на Windows
- Публикацию релизов через `softprops/action-gh-release@v2`

---
© 2026 xem1zo
