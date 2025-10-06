@4723217 ➜ /workspaces/WE-Docker (main) $ docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS                                         NAMES
df32346c2a44   we-docker-backend   "uvicorn main:app --…"   33 seconds ago   Up 32 seconds   0.0.0.0:8000->8000/tcp, [::]:8000->8000/tcp   we-docker-backend-1
89b6be191869   postgres:15         "docker-entrypoint.s…"   9 hours ago      Up 32 seconds   5432/tcp                                      we-docker-db-1