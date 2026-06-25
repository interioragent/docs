---
title: Installation
layout: home
nav_order: 2
---

# Installation

Start with cloning the github project

```bash
git clone https://github.com/KunalMGupta/interioragent.git
```

Create a conda envrionment
```bash
conda create -n interioragent python=3.12 -y
conda activate interioragent
```

Install dependencies
```bash
pip install numpy matplotlib trimesh scipy sceneprogllm
```

Make sure to follow instructions on [SceneProgExec](https://github.com/KunalMGupta/SceneProgExec.git) to correctly setup ```sceneprogexec``` package.

Download `datasets.zip` from [this OneDrive link](https://ucsdcloud-my.sharepoint.com/:u:/g/personal/k5gupta_ucsd_edu/IQA-MyG8SVWHQq4bWCD7amCmAWr9R9hyxe8e6udYgZNZ_TI?e=aX7HBn) and extract it into `interioragent/IDSDL` so the data lands at `IDSDL/datasets/assets/` and `IDSDL/datasets/futurehssd/`:
```bash
unzip datasets.zip -d IDSDL/
```

Setup your OPENAI_API_KEY
```bash
OPENAI_API_KEY="sk-..."
```

And you are done! Try running a creating a simple scene to see if it works.

```python
from IDSDL.scene import SceneProgRoom

scene = SceneProgRoom("test_room")

with scene.RelativeGroup() as seating_area:
    sofa = scene.AddAsset("a modern gray sofa")
    coffee_table = scene.AddAsset("a wooden coffee table")
    seating_area.set_anchor(sofa)
    seating_area.place_on_front(coffee_table)

with scene.RoomGroup() as main_room:
    main_room.place_on_center(seating_area)

scene.export("test_scene.blend")
```

```bash
python firstscene.py
```

<br>
<p style="text-align: center;">
  <img src="assets/installation.png" alt="medium-term" style="width: 80%;">
</p>
<br>

## Next step
Continue to [Getting Started](getting-started) for an overview on InteriorAgent-IDSDL. 