# uc3-present
Git based UC3 team presentations

## CDL Presentations
- [ECS Logging in Merritt with Fluent Bit](https://github.com/CDLUC3/uc3-present/blob/main/ecs_logging/README.md)
- [Rationale for Placement of Sceptre Properties](https://github.com/CDLUC3/uc3-present/tree/main/sceptre_properties#readme)

## UCTech 2025
- [ECS Logging in Merritt with Fluent Bit](ecs_logging)
- [On the Journey to DevOps, Start with Your Build](https://merritt.uc3dev.cdlib.org/present/devops_build/README.html)

## Team Presentations
- [ZooKeeper as a Queue and Lock Solution for Merritt](https://merritt.uc3dev.cdlib.org/present/zk/zk.html)

## UCTech 2024
- [OpenSearch For The Productivity Win; Fostering Adoption Across Teams](https://merritt.uc3dev.cdlib.org/present/opensearch/README.html)
- [Adopting Monthly Operational Review Meetings as a Learning Exercise](https://merritt.uc3dev.cdlib.org/present/monthly_ops/demo.html)

## Usage Notes

### Create Animated Gifs
```bash
brew install ffmpeg

ffmpeg -i input.mov -r 24 output.gif
```

### PDF Generation

```
docker run --rm -t --net=host -v .:/slides astefanutti/decktape https://merritt.uc3dev.cdlib.org/present/opensearch/README.html present.pdf
```

```
docker run --rm -t --net=host -v .:/slides astefanutti/decktape https://merritt.uc3dev.cdlib.org/present/monthly_ops/demo.html present.pdf
```

```
docker run --rm -t --net=host -v .:/slides astefanutti/decktape https://merritt.uc3dev.cdlib.org/present/devops_build/README.html present.pdf
```
