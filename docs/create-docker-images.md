# Lobsters Docker Image

## kelseyhightower/lobsters:1.0.0

```
git clone https://github.com/kelseyhightower/lobsters.git
```

```
cp Dockerfile lobsters
```

```
docker build -t kelseyhightower/lobsters:1.0.0 lobsters
```

```
docker push kelseyhightower/lobsters:1.0.0
```

## kelseyhightower/lobsters:1.1.0

```
cp custom.css lobsters/app/assets/stylesheets/local
```

```
docker build -t kelseyhightower/lobsters:1.1.0 lobsters
```

```
docker push kelseyhightower/lobsters:1.1.0
```

