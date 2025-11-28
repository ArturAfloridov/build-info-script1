import subprocess
import datetime
from pathlib import Path


def run(cmd):
    """Запуск Git-команды и вывод результата."""
    result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
    if result.stdout:
        print(result.stdout)
    if result.stderr:
        print(result.stderr)


def git_add():
    run("git add .")


def git_commit(message: str):
    run(f'git commit -m "{message}"')


def git_push():
    run("git push")


def show_last_modified():
    latest = max(Path(".").rglob("*"), key=lambda p: p.stat().st_mtime)
    mtime = datetime.datetime.fromtimestamp(latest.stat().st_mtime)
    print(f"\nПоследнее изменение файла: {latest}")
    print(f"Время изменения: {mtime.strftime('%Y-%m-%d %H:%M:%S')}\n")


def main():
    message = input("Введите commit message: ")

    print("\n--- Выполняю git add ---")
    git_add()

    print("\n--- Выполняю git commit ---")
    git_commit(message)

    print("\n--- Показываю время последнего изменения ---")
    show_last_modified()

    print("\n--- Выполняю git push ---")
    git_push()

    print("\nГотово! Всё отправлено на GitHub.")


if __name__ == "__main__":
    main()
