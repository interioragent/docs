---
title: Installation
layout: home
nav_order: 2
---

# Installation

1. Start with cloning the github project

```bash
git clone https://github.com/KunalMGupta/interioragent.git
```

2. Create a conda envrionment
```bash
conda create -n interioragent python=3.12 -y
conda activate interioragent
```

3. Install dependencies
```bash
pip install numpy matplotlib trimesh scipy sceneprogllm
```

4. Make sure to follow instructions on ```https://github.com/KunalMGupta/SceneProgExec.git``` to correctly setup ```sceneprogexec``` package.

5. Download and extract the ```datasets.zip``` to ```interioragent/IDSDL```
```bash
unzip dataset.zip
```

6. Setup your OPENAI_API_KEY
```bash
OPENAI_API_KEY="sk-..."
```

7. And you are done! Try running a simple program to see if it works. 

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

Next, let's [Get Started](getting-started) on learning more about InteriorAgent project and IDSDL. 