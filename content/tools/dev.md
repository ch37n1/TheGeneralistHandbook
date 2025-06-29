## Coding
- [astral-sh](https://docs.astral.sh/)
	- uv
	- ruff
- [golangci-lint](https://github.com/golangci/golangci-lint)
	- Musthave linters для go
- [wemake-python-styleguide](https://wemake-python-styleguide.readthedocs.io/en/latest/index.html)
	- Musthave linters для python
- [atlas](https://atlasgo.io/)
	- Language agnostic migration
- [gitleaks](https://gitleaks.io/) 
	- Сканер утечек секретов
- [cloc](https://github.com/AlDanial/cloc)
	- Статистика по файлам, полезно в [[GDD]]
- [shotgun](https://github.com/glebkudr/shotgun_code)
	- Утилита для формирования документации для [[GDD]] проектов
- IDE & agents
	- [zed](https://zed.dev/)
	- [aider](https://aider.chat/)
	- [cursor](https://www.cursor.com/)
- Interactive notebooks
	- Jupyter ([awesome](https://github.com/ml-tooling/best-of-jupyter))
	- [Marimo](https://marimo.io/)
Atlas:
```Makefile
mn: # New migration
	./scripts/new_migration.sh

mh: # Migrate hash
	atlas migrate hash --dir "file://${DATABASE_MIGRATIONS_DIR}"

ma: # Apply migrations
	atlas migrate apply --url ${DATABASE_URL}  --dir "file://${DATABASE_MIGRATIONS_DIR}"

md: # Drop migrations
	atlas migrate apply --url ${DATABASE_URL} --to 0  --dir "file://${DATABASE_MIGRATIONS_DIR}"

m-clear: # Clear migrations
	rm -rf ./deployments/migrations/*
```

**Linters:**
```Makefile
l:
	clear
	ruff check ./services ./services/* && ruff format
	go fmt ./services/...

dl: l # Deep lint
	uv run mypy ./services
	uv run flake8 ./services
	golangci-lint run ./services/...
```

**Tuna:**
`tuna http 8080 --subdomain=test-app`

**Leaks:**
`gitleaks git -v`

**Code-stats:**
`cloc . --exclude-dir=.venv,.git,_local,.pytest_cache,.ruff_cache,.vscode,uv.lock,.mypy_cache --by-file-by-lang`

