# Lobsters Docker Images

## kelseyhightower/lobsters:2.0.0

```
git clone https://github.com/kelseyhightower/lobsters.git
```

```
cp lobsters-on-kubernetes/Dockerfile lobsters/
```

```
docker build -t kelseyhightower/lobsters:2.0.0 lobsters
```

```
docker push kelseyhightower/lobsters:2.0.0
```

## kelseyhightower/lobsters:2.0.1

```
cp lobsters-on-kubernetes/custom.css lobsters/app/assets/stylesheets/local
```

```
docker build -t kelseyhightower/lobsters:2.0.1 lobsters
```

```
docker push kelseyhightower/lobsters:2.0.1
```

