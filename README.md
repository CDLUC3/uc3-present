# uc3-present
Git based UC3 team presentations

## UCTech 2024
- [OpenSearch For The Productivity Win; Fostering Adoption Across Teams](https://merritt.uc3dev.cdlib.org/present/opensearch/README.html)
- [Adopting Monthly Operational Review Meetings as a Learning Exercise](https://merritt.uc3dev.cdlib.org/present/monthly_ops/demo.html)
- [ZooKeeper as a Queue and Lock Solution for Merritt](https://merritt.uc3dev.cdlib.org/present/zk/zk.html)

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