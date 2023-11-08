
![DINO WARS](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/th5xamgrr6se0x5ro4g6.png)


# Dino Wars
Dino Wars is a game produced by me for my So_Long project at 42, Abu Dhabi. This project is a very small 2D game.
Its purpose is to make you work with textures, sprites,
and some other very basic gameplay elements.

## Installing

Clone the repository, make bonus and then run the executable file with the choice of your map in /map folder.

```bash
  cd so_long
  make bonus
  ./so_long_bonus ./map/map1_bonus.ber
```

## So_Long Guide
In this guide, I would be mainly focusing on creating the game logics, animations and how you can achieve anything using MiniLibX

#### Understanding MiniLibX
The best place to start would be [MiniLibX Docs](https://harm-smits.github.io/42docs/libs/minilibx), It can be very intimidating at first, but believe me you this tool is a wonder !

Refer to the following sections while going through the **MiniLibX Docs**


 

### Getting Started

* Create a new C program and try initializing MLX and a window

* The part where you create a new image using **mlx_new_image()** is not necessary, as per my eperience you will never be using it. Instead use **mlx_pixel_put()** to understand how you have control over the MLX window

* Concepts of **colors, little endian and big endian** were also not used in my project, only intitial testing and understanding of MLX involved **colors**.

* Your key takeaway from the Getting Started page of MiniLibX would be to succesfully understand the initialization of MLX and how you can control the graphics

#### Pixelput Project (Understanding Graphics)
This project will give you a very simple and easy idea about how you render images onto the MLX Window

The MLX window assigns the position of each pixel using X and Y position starting from the top left corner

![VISUAL_REPRESENTATION]()


    void render_square()
    {
        x = 350;
        y = 350;
        while(x < 450)
        {
            while(y < 450)
            {
            mlx_pixel_put(mlx, win, x, y, #FFFFFF);
            i = 20;
            y++;
            }
        y = 350;
        x++;
        }
    }

The above code simply goes through a loop and renders 1 pixel everytime with different X and Y position, resulting in a simple square in your Window 

![VISUAL_REPRESENTATION]()

Now that you're aware of how you have access to each and every pixel in your window lets go ahead and see how you can interact with your window

*The next section in the MiniLibX Docs about color will not be used, however its your choice to experiement on that, but I would suggest not to spend alot of time on that section*


### Loops, Hooks and Events

#### Loops
Loop refers to the infinite loop that goes on infinitely throughout your program. **mlx_loop()** is the perfect example, it keeps your program running infinitly unless **exit()** is initiated  

#### Hooks 
Hooks look for your input that you pass through the program, through your mouse or keyboard. MiniLibx has different functions for each input device. However the best way would be to use **mlx_hook()** and then assign events as per prototype. You'll learn about this more in a while.

#### Events

Events are just the input event that happens, refer to the table below for more information

### Integrating inputs

Integrating inputs are littl ricky, there are certain rules you have to go through

#### mlx_hook (mlx, win, key, mask, function, parameter)

The above is a simplified prototype of **mlx_hook**
* **Mlx and win** refer to the pointers

* **Key** refers to the input key that you want your program to be looking for (refer to table below)

* **Mask** refers to the key mask (not important, you can learn about this in the end)

* **Function** refers to the function you want to call when the key is presed, **your function prototype should match the prototype of the corresponding key** (refer to the table below)

* **Parameter** is the parameter you will be passing to the **Function** that is called.

If you notice the prototype column in the table below, the ***param** is the same parameter that is passed from **mlx_hook** to the function that is called.


| Key | Event      | Prototype |
| :--:| :-------:  | :---: |
| 2   | Key press   | int (*f)(int keycode, void *param) |
| 3   | Key release | 	int (*f)(int keycode, void *param) |
|4||Mouse Click| int (*f)(int button, int x, int y, void *param)|
|5| Mouse Release |int (*f)(int button, int x, int y, void *param)|
|6| Moving Mouse |int (*f)(int x, int y, void *param)|

This table is a mini version of the one you see on MinilibX, these are the onces that are good to start with and then you can progress with more advance keys depending on your project

### Examples

Lets say I want to quit the my program whenever there is **ESC** input. This is how my code would look like

#### Quitting the program
Header defines the keycode that your function will be looking for to quit the game

Its important to use a struct that has all the variables that you use in your program as you can only pass one parameter through your functions


    #define ESC = 53     (Defining the keycode of ESC)

    typefdef struct s_vars{
        void    *mlx;
        void    *win;
    }t_vars;
_________________________________________________

The main function starts with intializing mlx and a window, we set the **mlx_hook()** to look for buttons pressed by passsing the key as **2** (refer the event table) and mask 0 as we won't be needing that

We pass the **quit()** function as per the **prototype** (refer event table) with the parameter as **vars**


    
    int main()
    {
        t_vars  *vars;

	    vars->mlx = mlx_init();
	    vars->win = mlx_new_window(vars->mlx, 1920 , 1080 , "HELLO");

        mlx_hook(vars->win, 2, 0, quit, vars);
        mlx_loop(vars->mlx);
    }


___
When any button is pressed quit function initiates, but unless the key is **ESC** it won't quit

    int quit(int keycode, t_vars *vars)
        {
            if (keycode == ESC)
            {
                mlx_destroy_window(vars->mlx, vars->win);
                exit();
            }
        }


The above code should stop the program and exit 