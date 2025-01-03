# pygbag-builder
This is a module to support people who want to make browser game with pygame. Normaly, pygame won't work on web browser but by using pygbag(wasm for pygame), we can publish pygame programs on the internet. 

When we use pygbag, we should pay attention to some points.
 - name the main file as `main.py`
 - call main function with asyncio.run
 - insert await asyncio.sleep(0) into while loops
 - remake some functions into async functions

These points are not fatal probrem if there aren't many while loops. But when we make a little complex games, these points become cumbersome. So, this module aims to support making those game.(This module may not enough to support highly complex game...sorry.)

## Install

```sh
pip install pygbag-builder
```

## How To
### preparation
You should make folder for your project at first. The main file must be `main.py` and contain main function. 

These are sample programs. `main.py` can import other `.py`files. The only thing that you should care about is to make main function.

```python:main.py
import pygame, sys
import module
pygame.init()
screen = pygame.display.set_mode((512, 512))
pygame.display.set_caption('template')
clock = pygame.time.Clock()

class Class:
    def __init__(self):
        pass
    def loop(self):
        while True:
            break
instance = Class()

def loop():
    while True:
        break


def main():
    color = 0
    while True:
        screen.fill((color, color, color))
        color = min(255, color+1)
        loop()
        instance.loop()
        module.loop()
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
        pygame.display.update()
        clock.tick(30)


if __name__ == '__main__':
    main()
```

```python:module.py
def loop():
    while True:
        break
```
### commands
`pygbag-builder` offers these three commands
- pygbag_builder main_flow
- pygbag_builder make_repo
- pygbag_builder set_page

#### pygbag_builder main_flow
This is a command to create needed files for pygbag.

please use this command in your project folder
```sh
python -m pygbag_builder main_flow
```

By using this command, your python files are automatically modified like this.
```python:main.py
import asyncio
import pygame, sys
import module
pygame.init()
screen = pygame.display.set_mode((512, 512))
pygame.display.set_caption('template')
clock = pygame.time.Clock()


class Class:

    def __init__(self):
        pass

    async def loop(self):
        while True:
            await asyncio.sleep(0)
            break


instance = Class()


async def loop():
    while True:
        await asyncio.sleep(0)
        break


async def main():
    color = 0
    while True:
        await asyncio.sleep(0)
        screen.fill((color, color, color))
        color = min(255, color + 1)
        await loop()
        await instance.loop()
        await module.loop()
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
        pygame.display.update()
        clock.tick(30)


if __name__ == '__main__':
    asyncio.run(main())
```
```python:module.py
import asyncio


async def loop():
    while True:
        await asyncio.sleep(0)
        break
```

:::note warn
This part may not work compleately in complex program. In that case, please modify some parts manually.
:::

Also, this command creates 'pygbag.yml'(.github/workflows/pygbag.yml)

These files are put in `pygbag_builder_build` folder which created in your project folder.

#### pygbag_builder make_repo
This is a command to make repository automatically. **It uses GitHub API so you need GitHub token**. This command not only make repository but also runs workflow(pygbag_build).So, You Don't have to change settings. Instead, you have to get GitHub token with necessary permissions. You can get your token from Developer settings in Settings of your account.

##### Personal access tokens
[Repository access] Select `All repositories`

[Permissions]
These permissions must be `Read and Write`
- Actions
- Administration
- Contents
- Pages
- Webhooks
- Workflows 

please use this command in your project folder
```sh
python -m pygbag_builder make_repo
```
:::note warn
Depending on the timing of the process, sometimes the workflow may not execute properly, In that case, run this command again. You will get an error because the repository already exist, but no problem. Or you can run `pygbag_build` manually from Action.
:::

If you don't want to use token. You have to create repository manually.

https://pygame-web.github.io/wiki/publishing/github.io/

summary
- run pygbag_build from Action
- set gh-pages to branch from Settings → pages

#### pygbag_builder set_page
This is a command to set `gh-pages` to branch. It also uses GitHub API.
Before using this command, you must check whether pygbag_build workflow is finished.(You can check it from Action)

please use this command in your project folder
```sh
python -m pygbag_builder set_page
```

If you want to test on https://localhost:8000 before uploading your game, you can use this command.
```sh
pygbag pygbag_builder_build
```



## License
This project follows MIT. If you need more information, please look at `LICENSE`.

## pygbag
https://pypi.org/project/pygbag/