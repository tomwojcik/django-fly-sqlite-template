.PHONY: up shell up-docker deps upgrade-deps deploy fly-shell

up:
	python src/manage.py runserver_plus 0.0.0.0:8000

shell:
	python src/manage.py shell_plus --ipython

up-docker:
	docker build -t {{cookiecutter.project_slug}} .
	docker run -t -i -p 8000:8000 {{cookiecutter.project_slug}}

deps:
	pip-compile requirements/requirements.in
	pip-compile -o requirements/requirements-dev.txt requirements/requirements.txt requirements/requirements-dev.in

upgrade-deps:
	pip-compile --upgrade requirements/requirements.in
	pip-compile --upgrade -o requirements/requirements-dev.txt requirements/requirements.txt requirements/requirements-dev.in

deploy:
	flyctl deploy --local-only

fly-shell:
	flyctl ssh console --command 'python /opt/app/manage.py shell'
