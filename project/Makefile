SHELL=cmd.exe
FRONT_END_BINARY=frontApp.exe
BROKER_BINARY=brokerApp

## up: starts all containers in the background without forcing build
up:
	@echo Starting Docker images...
	docker-compose up -d
	@echo Docker images started!

## up_build: stops docker-compose (if running), builds all projects and starts docker compose
up_build: build_broker
	@echo Stopping docker images (if running...)
	docker-compose down
	@echo Building (when required) and starting docker images...
	docker-compose up --build -docker
	@echo Docker images built and started!

## down: stop docker compose
down:
	@echo Stopping docker compose...
	docker-compose down
	@echo Done!

## build_broker: builds the broker binary as a linux executable
build_broker:
	@echo Building broker binary...
	chdir ..\broker-service\cmd\api && set GOOS=linux&& set GOARCH=amd64&& set CGO_ENABLED=0 && go build
	@echo Done!

## build_front: builds the front end binary
build_front:
	@echo Building front end binary...
	chdir ..\front-end\cmd\web && set CGO_ENABLED=0&& set GOOS=windows&& go build -o ${FRONT_END_BINARY}
	@echo Done!

## start: starts the front end
start: build_front
	@echo Starting front end
	chdir ..\front-end && start /B ${FRONT_END_BINARY} &

## stop: stop the front end
stop:
	@echo "Stopping back end..."
	@-pkill -SIGTERM -f "./frontApp"
	@echo "Stopped back end!"

test:
	@echo "Testing..."
	go test -v ./...