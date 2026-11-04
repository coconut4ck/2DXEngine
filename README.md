Hi! Thanks for downloading my first 2D engine written in C++. 2DXEngine is a lightweight and simple engine designed for 2D games and applications.

---

## 0.1 — ENGINE CORE

Engine components:
- Panels with texture, solid color, and shader panels
- Text
- Audio
- Regular buttons and texture buttons

Engine structure:

```
core
├── _libs     — libraries that MUST be in this exact folder,
│               as they play a key role in the project's operation on 2DXEngine
├── _main     — all the important scripts that make the engine work at all
├── _other    — other, but no less important files that handle rendering
├── _ui       — all UI objects listed above
├── _shaders  — shaders for UI objects
├── _plugins  — helper scripts that simplify your code
└── _res      — various resources that can be used for the project
```

---

## 0.2 — PROJECT STRUCTURE

To write a working project, you first need to know a few things. The project structure must always look like this:

```
project root folder
├── _assets              — all resources, including scripts
│   ├── _fonts
│   ├── _scripts
│   ├── _textures
│   ├── _audio
│   └── _...
├── _core                — engine core
├── _files               — compiled project files (EVERY FILE IN THIS
│                          FOLDER IS IMPORTANT FOR STABLE PROJECT OPERATION)
└── _project.2dxp        — project file (needed to open projects in the IDE)
```

---

## 1.0 — INSTALLING THE C++ COMPILER (CLANG) AND BASIC C++ LIBRARIES (C++17)

The engine is built using clang-cl (clang.exe), so before anything else you need to install LLVM/Clang and the basic C++ libraries (the main standard is C++17).

> If you already have clang and C++17 installed, you can skip this section.

### 1.0.1 — Installing LLVM/Clang

- Go to https://github.com/llvm/llvm-project/releases and download the LLVM installer for Windows (a file like `LLVM-XX.X.X-win64.exe`).
- Run the installer and go through it to the end.
- You MUST check the box "Add LLVM to the system PATH for all users" (or "for current user") so that `clang.exe` becomes available from the command line.
- After installation, open a new command prompt and type:

```
clang --version
```

- If the command works and shows a version — clang is installed correctly. If not — reboot your computer or check PATH manually.

### 1.0.2 — Installing Basic C++ Libraries (C++17)

- Download "Build Tools for Visual Studio" from https://visualstudio.microsoft.com/visual-cpp-build-tools/
- Run the installer and check "Desktop development with C++" in the component list.
- In the right panel "Installation details", make sure the following are checked:
  - MSVC v143 — VS 2022 C++ x64/x86 build tools (or the current version)
  - Windows 10/11 SDK
  - C++ CMake tools for Windows (optional)
- Click "Install" and wait for completion.

This gives you the standard C++ library (STL) and header files. The C++17 standard is set separately in the build settings in the IDE (section 2.4.2 — "C++ standard").

- Verify the installation: open a command prompt and type:

```
cl
```

- If the command is not found — run it through "x64 Native Tools Command Prompt for VS 2022".

> IMPORTANT: if you needed git bash to install LLVM/Clang or Build Tools — then first complete section 1.1, then return to sections 1.0.1 and 1.0.2. In most cases git bash is not needed to install clang and C++.

---

## 1.1 — INSTALLING GIT BASH

> If you already have it, you can skip this section.

Go to https://git-scm.com/install/windows and download git bash.

---

## 1.2 — INSTALLING VCPKG

> If you already have it, you can skip this section.

Now that you have git bash, run this command in the command prompt:

```
git clone https://github.com/microsoft/vcpkg.git
```

Then go to the folder where vcpkg was downloaded. Find the `bootstrap-vcpkg.bat` file in it and run it.

Also don't forget to add vcpkg to PATH:

- Press the WIN+R hotkey
- In the appeared line, type `sysdm.cpl`
- In the new window that appears, find and click the "Advanced" button at the top
- Then click the "Environment Variables" button at the bottom
- In another window that appears, find the "User variables for ..." section
- Just below, find the "New" button under this section, and in the newly opened window there will be 2 lines: "Name" and "Value"
- In the line next to "Name", enter the variable name — `VCPKG_ROOT`
- In the line next to "Value", enter the path (the path depends on where you placed the vcpkg folder, e.g. `C:\vcpkg`)
- Now click OK and you can exit, saving PATH

---

## 1.3 — INSTALLING DEPENDENCIES IN VCPKG

> If you already have them, you can skip this section.

Once you've done everything in section 1.2, it's time to install the dependencies so the engine works.

- Open a command prompt and type `vcpkg`. If the command works, you did everything correctly, and now type the command:

```
vcpkg install glfw3:x64-windows glm:x64-windows freetype:x64-windows
```

When everything is installed, check that the dependencies are present:

```
vcpkg list
```

---

That's it! Now that you've installed everything, we can explain how to write your first project.

## 2.1 — PROJECT PREPARATION

- Open the IDE file called `2DXEngine.exe`
- Once you're in it, you'll see:
  - A field for scripts
  - A file tree
  - A message output field (e.g., during compilation)
  - A top panel with various buttons (File, Edit, Project, View...)

To create your first project, click the File button on the top panel, then click "New project" in the menu that appears.

Choose the folder where the project will be located, and the project will be created! It will open immediately after creation.

In the file tree, create a `scripts` folder where all future scripts will be stored.

In this same folder, create 2 files — `main.h` and `main.cpp` (you can use your own file names instead of `main`).

---

## 2.2 — CREATING THE PROJECT

### 2.2.1 — Basic Code in main.h

```cpp
#pragma once
#include <core/main/scenes/scene.h> // Import important
#include <core/main/includes.h>     // libraries (you can also import other C++ libraries)


class Main : public Scene {
public:
    const char* name() const override { return "Main"; }
    void onEnter() override;
    void onUpdate(float dt) override;
    void onDraw() override;
    void onExit() override;
private:
    here you will declare all objects and variables
};
```

Where `class Main` and `{ return "Main"; }` -> `Main` <- is the scene name.

The functions `onEnter()`, `onUpdate()`, `onDraw()`, `onExit()` are mandatory functions that must never be removed: the code is executed through them.

### 2.2.2 — How onEnter(), onUpdate(), onDraw(), onExit() Work

- `onEnter()` — runs when the scene starts
- `onUpdate(float dt)` — runs while the scene is active
- `onDraw()` — runs when the scene is rendered
- `onExit()` — runs when exiting the scene

And here's what `main.cpp` looks like:

```cpp
#include "main.h" // path to main.h
#include <iostream>                 // Import important
#include <core/main/includes.h>     // libraries (you can also import other C++ libraries)


void Main::onEnter() {
}

void Main::onUpdate(float dt) {
}

void Main::onDraw() {
}

void Main::onExit() {
}

REGISTER_SCENE(Main); // The scene is created
```

---

### 2.2.3 — UI Objects and Their Properties

#### 2.2.3.1 — Creating UI Objects

In `main.h`, the class has 2 categories: `public` and `private`. So, under `public` and everything below it, you need to write `private:` and under it create your variables and objects.

Below is an example of how all objects are created:

```cpp
#pragma once
#include <core/main/scenes/scene.h>
#include <core/main/includes.h>


class Main : public Scene {
public:
    const char* name() const override { return "Main"; }
    void onEnter() override;
    void onUpdate(float dt) override;
    void onDraw() override;
    void onExit() override;
private:
    ColorPanel color_panel; // after the class name comes the object name (and so on for all objects — Class name;)
    TexturePanel texture_panel;
    ShaderPanel shader_panel;
    
    ColorButton color_button;
    TextureButton texture_button;

    Text text;


    // Also plugins that can be used in the project
    Sound sound;
    DeltaTime delta;
    Random random;
    Timer timer;
    FpsCounter fps;
};
```

#### 2.2.3.2 — Using Objects

For objects to appear on the scene, they must be drawn in `onDraw()` and created in `onEnter()` with the required properties set.

Here are all the object properties and how to write them correctly in `main.cpp`:

```cpp
#include "main.h"
#include <iostream>
#include <core/main/includes.h>


void Main::onEnter() {
    color_panel.visible = true;
    color_panel.position = glm::vec2(0.0f, 0.0f);
    color_panel.rotation = 0.0f;
    color_panel.scale = glm::vec2(100.0f, 100.0f);
    color_panel.color = glm::vec3(1.0f, 1.0f, 1.0f);
    color_panel.alpha = 1.0f;
    color_panel.show();
    color_panel.hide();
    color_panel.create(glm::vec3(1.0f, 0.5f, 0.2f));

    texture_panel.visible = true;
    texture_panel.position = glm::vec2(0.0f, 0.0f);
    texture_panel.rotation = 0.0f;
    texture_panel.scale = glm::vec2(100.0f, 100.0f);
    texture_panel.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    texture_panel.alpha = 1.0f;
    texture_panel.setTexture("assets/textures/example.png");
    texture_panel.getTexture();
    texture_panel.show();
    texture_panel.hide();
    texture_panel.create("assets/textures/example.png");

    shader_panel.visible = true;
    shader_panel.position = glm::vec2(0.0f, 0.0f);
    shader_panel.rotation = 0.0f;
    shader_panel.scale = glm::vec2(100.0f, 100.0f);
    shader_panel.alpha = 1.0f;
    shader_panel.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    shader_panel.getShader();
    shader_panel.show();
    shader_panel.hide();
    shader_panel.create("assets/shaders/shader_vertex.glsl", "assets/shaders/shader_fragment.glsl");

    text.visible = true;
    text.position = glm::vec2(0.0f, 0.0f);
    text.rotation = 0.0f;
    text.scale = 1.0f;
    text.alpha = 1.0f;
    text.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    text.text = "Hello";
    text.pixelSize = 48;
    text.setText("Hello World");
    text.show();
    text.hide();
    text.create("Hello", "core/res/fonts/font1.ttf");

    color_button.visible = true;
    color_button.position = glm::vec2(0.0f, 0.0f);
    color_button.rotation = 0.0f;
    color_button.scale = glm::vec2(100.0f, 100.0f);
    color_button.color = glm::vec3(1.0f, 1.0f, 1.0f);
    color_button.alpha = 1.0f;
    color_button.text = "Button";
    color_button.text_scale = 1.0f;
    color_button.pixelSize = 48;
    color_button.lineSpacing = 0.0f;
    color_button.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    color_button.show();
    color_button.hide();
    color_button.create(glm::vec3(1.0f, 0.5f, 0.2f), "Button", "assets/fonts/font1.ttf");

    texture_button.visible = true;
    texture_button.position = glm::vec2(0.0f, 0.0f);
    texture_button.rotation = 0.0f;
    texture_button.scale = glm::vec2(100.0f, 100.0f);
    texture_button.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    texture_button.alpha = 1.0f;
    texture_button.show();
    texture_button.hide();
    texture_button.create("assets/textures/example.png");
}

void Main::onUpdate(float dt) {
    color_panel.visible = true;
    color_panel.position = glm::vec2(0.0f, 0.0f);
    color_panel.rotation = 0.0f;
    color_panel.scale = glm::vec2(100.0f, 100.0f);
    color_panel.color = glm::vec3(1.0f, 1.0f, 1.0f);
    color_panel.alpha = 1.0f;
    color_panel.show();
    color_panel.hide();

    texture_panel.visible = true;
    texture_panel.position = glm::vec2(0.0f, 0.0f);
    texture_panel.rotation = 0.0f;
    texture_panel.scale = glm::vec2(100.0f, 100.0f);
    texture_panel.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    texture_panel.alpha = 1.0f;
    texture_panel.setTexture("assets/textures/example.png");
    texture_panel.getTexture();
    texture_panel.show();
    texture_panel.hide();

    shader_panel.visible = true;
    shader_panel.position = glm::vec2(0.0f, 0.0f);
    shader_panel.rotation = 0.0f;
    shader_panel.scale = glm::vec2(100.0f, 100.0f);
    shader_panel.alpha = 1.0f;
    shader_panel.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    shader_panel.getShader();
    shader_panel.show();
    shader_panel.hide();

    text.visible = true;
    text.position = glm::vec2(0.0f, 0.0f);
    text.rotation = 0.0f;
    text.scale = 1.0f;
    text.alpha = 1.0f;
    text.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    text.text = "Hello";
    text.pixelSize = 48;
    text.setText("Hello World");
    text.show();
    text.hide();

    color_button.visible = true;
    color_button.position = glm::vec2(0.0f, 0.0f);
    color_button.rotation = 0.0f;
    color_button.scale = glm::vec2(100.0f, 100.0f);
    color_button.color = glm::vec3(1.0f, 1.0f, 1.0f);
    color_button.alpha = 1.0f;
    color_button.text = "Button";
    color_button.text_scale = 1.0f;
    color_button.pixelSize = 48;
    color_button.lineSpacing = 0.0f;
    color_button.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    color_button.show();
    color_button.hide();
    color_button.is_hovered();
    color_button.is_pressed();
    color_button.is_click();

    texture_button.visible = true;
    texture_button.position = glm::vec2(0.0f, 0.0f);
    texture_button.rotation = 0.0f;
    texture_button.scale = glm::vec2(100.0f, 100.0f);
    texture_button.tint = glm::vec3(1.0f, 1.0f, 1.0f);
    texture_button.alpha = 1.0f;
    texture_button.show();
    texture_button.hide();
    texture_button.is_hovered();
    texture_button.is_pressed();
    texture_button.is_click();
}

void Main::onDraw() {
    color_panel.draw();

    texture_panel.draw();

    shader_panel.draw();

    text.draw();

    color_button.draw();

    texture_button.draw();
}

void Main::onExit() {
    color_panel.hide();
    color_panel.destroy();

    texture_panel.hide();
    texture_panel.destroy();

    shader_panel.hide();
    shader_panel.destroy();

    text.hide();
    text.destroy();

    color_button.hide();
    color_button.destroy();

    texture_button.hide();
    texture_button.destroy();
}

REGISTER_SCENE(Main);
```

#### 2.2.3.3 — Using Plugins

Plugins work exactly the same way, but have fewer properties:

```cpp
#include "main.h"
#include <iostream>
#include <core/main/includes.h>


void Main::onEnter() {
    delta_time.get_delta();
    delta_time.get_physic_delta();

    fps_counter.get_fps_float(1.0f);
    fps_counter.get_fps_int(1.0f);

    random.random_float(0.0f, 1.0f);
    random.random_int(0, 100);
    random.random_bool();

    sound.play("core/res/sounds/example.wav", false, false);
    sound.play("core/res/sounds/example.wav", true, true);
    sound.stop();

    timer.start(1.0f, false);
    timer.start(1.0f, true);
    timer.stop();
    timer.update(delta_time.get_delta());
    timer.finished();
    timer.get_current_time();
    timer.set_start_time(2.0f);
}

void Main::onUpdate(float dt) {
    float delta = delta_time.get_delta();
    float physic_delta = delta_time.get_physic_delta();

    float fps_f = fps_counter.get_fps_float(1.0f);
    int fps_i = fps_counter.get_fps_int(1.0f);

    float rnd_f = random.random_float(0.0f, 1.0f);
    int rnd_i = random.random_int(0, 100);
    bool rnd_b = random.random_bool();

    sound.play("assets/sounds/example.wav", false, false);
    sound.stop();

    timer.update(delta);
    timer.update(physic_delta);
    timer.finished();
    timer.get_current_time();
    timer.set_start_time(2.0f);
    timer.start(1.0f, false);
    timer.stop();
}

void Main::onDraw() {
    float delta = delta_time.get_delta();
    float physic_delta = delta_time.get_physic_delta();

    float fps_f = fps_counter.get_fps_float(1.0f);
    int fps_i = fps_counter.get_fps_int(1.0f);

    float rnd_f = random.random_float(0.0f, 1.0f);
    int rnd_i = random.random_int(0, 100);
    bool rnd_b = random.random_bool();

    sound.play("assets/sounds/example.wav", false, false);
    sound.stop();

    timer.update(delta);
    timer.update(physic_delta);
    timer.finished();
    timer.get_current_time();
    timer.set_start_time(2.0f);
    timer.start(1.0f, false);
    timer.stop();
}

void Main::onExit() {
    sound.stop();

    timer.stop();
}

REGISTER_SCENE(Main);
```

---

### 2.2.4 — Description of All UI Objects

Here's a brief description of what each engine UI object does and what it's for.

#### 2.2.4.1 — ColorPanel

A rectangle filled with a solid color. Used as a background, backing, frame, or a simple graphical primitive.

#### 2.2.4.2 — TexturePanel

A rectangle with an image (texture) applied to it. Used for sprites, backgrounds, pictures, etc.

#### 2.2.4.3 — ShaderPanel

A rectangle rendered with its own shader (vertex and fragment). Used for effects, animations, and non-standard graphics.

Additionally, you can pass your own uniform variables to the shader via `setFloat`, `setVec2`, `setVec3`, `setVec4`, `setInt`.

The `time` field is convenient for animations — you can increment it in `onUpdate` via `update(dt)` and pass it to the shader.

#### 2.2.4.4 — Text

Displays text with the font you need.

#### 2.2.4.5 — ColorButton

A button with a solid color and text. It reacts to cursor hover, press, and click.

#### 2.2.4.6 — TextureButton

A button with an image instead of a solid color. It also reacts to hover, press, and click.

#### 2.2.4.7 — Common Methods and Properties of UI Objects

Most UI objects share common properties:

- `position` (glm::vec2) — position in virtual coordinates
- `rotation` (float) — rotation in degrees
- `scale` — size (glm::vec2 for panels and buttons, float for text)
- `alpha` (float) — opacity from 0.0f to 1.0f
- `visible` (bool) — object visibility

And common methods:

- `show()` / `hide()` — show/hide the object
- `draw()` — render the object (called in `onDraw()`)
- `destroy()` — free resources (called mostly in `onExit()`)

#### 2.2.4.8 — Buttons: is_hovered(), is_pressed(), is_click()

- `is_hovered()` — returns true if the cursor is over the button (taking rotation and scale into account)
- `is_pressed()` — returns true if the left mouse button is held down and the cursor is over the button
- `is_click()` — returns true once at the moment of a click (press + release), convenient for handling actions

---

## 2.3 — BUILDING THE PROJECT

The project is done, now it needs to be compiled so you can run it.

To do this, in the IDE click the "Project" button on the top panel. In the menu that appears, click "Build" to compile the project, "Run" to run the project (if it's compiled), or "Build and run" to compile and immediately run the project.

If any errors appear, the output field will show you everything.

---

## 2.4 — IDE SETTINGS

The IDE also has very important settings. To access them, find and click the "Project" button on the top panel, then click "Project settings" in the menu that appears.

In the IDE settings there are 2 sections: project settings and project build settings.

### 2.4.1 — Project Settings

It's all simple here:

- **Title** — project name
- **width** and **height** — the dimensions of the project WINDOW
- **v_width** and **v_height** — the virtual resolution of the project, i.e., whatever the window size is, the resolution in this window will always be the same
- **fullscreen** — fullscreen mode
- **main_scene** — you need to specify the name of the scene the game will start with

### 2.4.2 — Project Build Settings

This is more complicated:

- **Save project before build** — save the project before building
- **VCPKG path** — path to `vcpkg\installed\x64-windows`, e.g. `C:\vcpkg\installed\x64-windows`
- **clang-cl path** — path to `clang.exe`; since the build is done with it, you need to use this path (in general, you can just write `clang.exe` in the field; if that doesn't work, you need to write a relative path from `C:\`)
- **C++ standard** — there are several C++ versions to choose from, but the main one is C++17
- **Output exe name** — what the output project file should be named (e.g. `example.exe`)
- **Subsystem (WINDOWS or CONSOLE)** — roughly speaking, whether the console is present: in CONSOLE mode, when the exe application starts, a command prompt will appear with output (e.g. errors or console prints) — very good for testing. And in WINDOWS mode the console doesn't appear on startup
- **Extra libs** and **Extra includes** — additional paths for compilation
- **Show build output** — if checked, the compilation output is shown during the build

---

Now that you've figured out the engine — start creating!