# SmolVLM2-500M NPU
![Alt text](https://github.com/user-attachments/assets/6d297a34-c516-4cb1-be4a-bca471d40fa6)
<br><br>**User**:\<image\>Describe the image.<br><br>
**Answer**: The image is a surreal and fantastical representation of a space station orbiting a planet in outer space. The station, which appears to be a large, cylindrical structure with multiple levels, is depicted as being made entirely of ice and water. The station's surface is covered in a thick layer of ice, creating a stark contrast against the barren landscape below.

The planet itself is a dark, icy world, with no visible signs of life or vegetation. However, there are several large, glowing orbs scattered across the surface, which seem to be floating in mid-air. These orbs could be either stars or other celestial bodies, adding an element of mystery and wonder to the scene.

The station's interior is also depicted as being made entirely of ice and water, with no visible doors or windows. The walls are covered in a thick layer of ice, creating a sense of isolation and confinement. However, there are several small, glowing orbs scattered throughout the interior, which could be either stars or other celestial bodies, adding an element of mystery and wonder to the scene.

The overall composition of the image creates a sense of awe and wonder, as if the viewer is witnessing a scene from a science fiction movie or a fantasy novel. The use of ice and water as the primary material for the station's structure adds a unique and otherworldly element to the scene. The glowing orbs could be either stars or other celestial bodies, adding an element of mystery and wonder to the scene.

Overall, the image is a beautiful and imaginative representation of a space station orbiting a planet in outer space, with ice and water as its primary materials. It creates a sense of isolation and confinement, while also adding an element of mystery and wonder to the scene. The use of ice and water as the primary material for the station's structure adds a unique and otherworldly element to the scene. The glowing orbs could be either stars or other celestial bodies, adding an element of mystery and wonder to the scene. The overall composition of the image creates a sense of awe and wonder, as if the viewer is witnessing a scene from a science fiction movie or a fantasy novel.

------------

## SmolVLM2-500M VLM for RK3588 NPU (Rock 5, Orange Pi 5). <br/>
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>
Paper: https://huggingface.co/blog/smolvlm2
Hugging face: https://huggingface.co/blog/smolvlm2

------------

## Introduction

LLMs (Large Language Models) are neural networks trained on extensive text datasets to comprehend and produce language.<br>
VLMs (Vision-Language Models) incorporate a visual encoder, allowing the model to process images and text simultaneously.<br> 
A combined VLM+LLM system is often referred to as a multimodal model.

These models can be large—hundreds of millions to billions of parameters—which impacts accuracy, memory use, and runtime speed.<br>
On edge devices like the RK3588, available RAM and compute are limited, and even the NPU has strict constraints on supported operations.<br>
Because of this, models typically need to be quantised or simplified to fit.

Performance is usually expressed in tokens (words) per second.<br>
Once converted to RKNN, parts of the model can run on the NPU, improving speed.<br>

------------

## Model performance benchmark (FPS)

All models, with C++ examples, can be found on the Q-engineering GitHub.<br><br>
All LLM models are quantized to **w8a8**, while the VLM vision encoders use **fp16**.<br>

| model         | RAM (GB)<sup>1</sup> | llm cold sec<sup>2</sup> | llm warm sec<sup>3</sup> | vlm cold sec<sup>2</sup> | vlm warm sec<sup>3</sup> | Resolution | Tokens/s |
| --------------| :--: | :-----: | :-----: | :--------: | :-----: | :--------:  | :--------: |
| [Qwen2-7B](https://github.com/Qengineering/Qwen2-7B-NPU) | 8.7 | 86.6 |   34.5 | 37.1  | 20.7 | 392 x 392 | 3.7 |
| [Qwen2-2.2B](https://github.com/Qengineering/Qwen2-2B-NPU) | 3.3 | 29.1 |   2.5 | 17.1  | 1.7 | 392 x 392 | 12.5 |
| [InternVL3-1B](https://github.com/Qengineering/InternVL3-NPU) | 1.3 |  6.8 |   1.1 | 7.8    | 0.75 | 448 x 448 | 30 |
| [SmolVLM2-2.2B](https://github.com/Qengineering/SmolVLM2-2B-NPU) | 3.4 | 21.2 |   2.6 | 10.5   | 0.9  | 384 x 384 | 11 |
| [SmolVLM2-500M](https://github.com/Qengineering/SmolVLM2-500M-NPU) | 0.8 |  4.8 |   0.7 | 2.5    | 0.25 | 384 x 384 | 31 |
| [SmolVLM2-256M](https://github.com/Qengineering/SmolVLM2-256M-NPU) | 0.5 |  1.1 |   0.4 | 2.5    | 0.25 | 384 x 384 | 54 |

<sup>1</sup> The total used memory; LLM plus the VLM. <br>
<sup>2</sup> When an llm/vlm model is loaded for the first time from your disk to RAM or NPU, it is called a cold start.<br>
The duration depends on your OS, I/O transfer rate, and memory mapping.<br> 
<sup>3</sup> Subsequent loading (warm start) takes advantage of the already mapped data in RAM. Mostly, only a few pointers need to be restored.<br><br>
<img width="600" height="450" alt="Figure_1" src="https://github.com/user-attachments/assets/fac39e85-3f6c-422a-82bc-3d7e4c62cf17" />

------------

## Dependencies.
To run the application, you have to:
- OpenCV 64-bit installed.
- rkllm library.
- rknn library.
- Optional: Code::Blocks. (```$ sudo apt-get install codeblocks```)

### Installing the dependencies.
Start with the usual 
```
$ sudo apt-get update 
$ sudo apt-get upgrade
$ sudo apt-get install cmake wget curl
```
#### OpenCV
To install OpenCV on your SBC, follow the Raspberry Pi 4 [guide](https://qengineering.eu/install-opencv-on-raspberry-64-os.html).<br><br>
Or, when you have no intentions to program code:
```
$ sudo apt-get install lib-opencv-dev 
```
------------

## Installing the app.
```
$ git clone https://github.com/Qengineering/SmolVLM2-500M-NPU.git
```

#### RKLLM, RKNN
To run SmolVLM2-500M, you need to have the **rkllm-runtime** library version **1.2.2** (or higher) installed, as well as the **rknpu driver** version **0.9.8**.<br>
If you don't have these on your machine, or if you have a lower version, you need to install them.<br>
We have provided the correct versions in the repo.<br>
```bash
$ cd ./SmolVLM2-500M-NPU/aarch64/library
$ sudo cp ./*.so /usr/local/lib
$ cd ./SmolVLM2-500M-NPU/aarch64/include
$ sudo cp ./*.h /usr/local/include
```
### Download the LLM and VLM model.
The next step is downloading the models.<br>
Download the two files (700 MB) from our Sync.com server:<br>
[smolvlm2-2.2b-instruct_w8a8_rk3588.rkllm](https://ln5.sync.com/dl/2ac529d30#49g27fih-qe8mjmu9-prmas7px-uuyua5te) and [smolvlm2-2.2b_vision_fp16_rk3588.rknn](https://ln5.sync.com/dl/b565d2360#wmbmdbum-tk36pehc-5t4irskd-7gb6kfti)<br>
Copy both to your `./model` folder.

### Building the app.
Once you have the two models, it is time to build your application.<br>
You can use **Code::Blocks**.
- Load the project file *.cbp in Code::Blocks.
- Select _Release_, not Debug.
- Compile and run with F9.
- You can alter command line arguments with _Project -> Set programs arguments..._ 

Or use **Cmake**.
```
$ mkdir build
$ cd build
$ cmake ..
$ make -j4
```
### Running the app.
The app has the following arguments.
```bash
VLM_NPU Picture RKNN_model RKLLM_model NewTokens ContextLength
```
| Argument   | Comment |
| --------------| --  |
| picture | The image. Provide a dummy if you don't want to use an image | 
| RKNN_model | The visual encoder model (vlm) | 
| RKLLM_model | The large language model (llm) | 
| NewTokens | This sets the maximum number of new tokens. Optional, default 2048| 
| ContextLength | This specifies the maximum total number of tokens the model can process. Optional, default 4096| 

<br>In the context of the Rockchip RK3588 LLM (Large Language Model) library, the parameters NewTokens and ContextLength both control different limits for text generation, and they're typical in LLM workflows.<br>
**NewTokens**<br> 
This sets the maximum number of tokens (pieces of text, typically sub-word units) that the model is allowed to generate in response to a prompt during a single inference round. For example, if set to 300, the model will not return more than 300 tokens as output, regardless of the prompt length. It's important for controlling generation length to avoid too-short or too-long responses, helping manage resource use and output size.<br>
**ContextLength**<br>
This specifies the maximum total number of tokens the model can process in one go, which includes both the prompt (input) tokens and all generated tokens. For example, if set to 2048 and your prompt already uses 500 tokens, the model can generate up to 2048-500 = 1548 new tokens. This is a hardware and architecture constraint set during model conversion and deployment, as the context window cannot exceed the model's design limit (for instance, 4096 or 8192 tokens depending on the model variant).

A typical command line can be:
```bash
VLM_NPU ./Moon.jpg ./models/SmolVLM2-2B-1b_vision_fp16_rk3588.rknn ./models/SmolVLM2-2B-1b_w8a8_rk3588.rkllm 2048 4096
```
The NewTokens (2048) and ContextLength (4096) are optional and can be omitted.
### Using the app.
Using the application is simple. Once you provide the image and the models, you can ask everything you want.<br>
Remember, we are on a bare Rock5C with a tiny model, so don't expect the same quality answers as ChatGPT can provide.<br>
If you want more sensible output, you'd better use a larger model like [InternVL3](https://github.com/Qengineering/InternVL3-NPU) or [Qwen](https://github.com/Qengineering/Qwen2-NPU).<br>
If you like the response to include a discussion of the picture, include the `<image>` token once in your prompt.<br>
The app remembers the dialogue until you give the token `<clear>`.<br>
With `<exit>`, you leave the application.
### C++ code.  
Below, you find the surprisingly little code of main.cpp. 
```cpp
#include "RK35llm.h"

int main(int argc, char** argv)
{
    std::string input_str;
    std::string output_str;
    RK35llm RKLLM;

    RKLLM.SetInfo(true);            //yes, you may give me additional model information
    RKLLM.SetSilence(false);        //you may print the incremental text chunks on the terminal

    if     (argc< 4) {std::cerr << "Usage: " << argv[0] << " image vlm_model llm_model [option]NewTokens [option]ContextLength\n"; return -1;}
    else if(argc==4) RKLLM.LoadModel(argv[2],argv[3]);
    else if(argc==5) RKLLM.LoadModel(argv[2],argv[3],std::atoi(argv[4]));
    else if(argc> 5) RKLLM.LoadModel(argv[2],argv[3],std::atoi(argv[4]),std::atoi(argv[5]));

    cv::Mat Pic = cv::imread(argv[1]);
    RKLLM.LoadImage(Pic);

    while(true) {
        printf("\n");
        printf("User: ");

        std::getline(std::cin, input_str);
        if (input_str == "exit") break;

        output_str = RKLLM.Ask(input_str);
//        std::cout << "\nLLM Reply: " << output_str << std::endl;
    }

    return 0;
}
```
Most code speaks for itself. One remark.<br>
The LLM generates the answer to your request in little pieces of text. You see them as if the LLM is typing on your terminal.<br>
RKLLM.SetSilence controls this behaviour. When set to `true`, it will suppress this output. RKLLM.Ask() also returns the complete answer.<br>
In main.cpp uncomment this line:
```cpp
std::cout << "\nLLM Answer: " << output_str << std::endl;
```

------------

## VLM examples.
| Image         | Dialog |
| --------------| --  |
| ![Singapore_S](https://github.com/user-attachments/assets/e742059a-3bdb-41be-9647-e7767da12ce6) |**User**: \<image\>Which city is this?<br>**Answer**: The image depicts the iconic Ferris wheel at the Ferris Wheel in San Francisco, California. The Ferris wheel is a large, circular structure that rotates to provide passengers with panoramic views of the city's skyline and surrounding waterfront area. It is one of the most recognizable landmarks in the world and has been a part of San Francisco's skyline since its construction in 1912.<br><br>The image shows the Ferris wheel from a low angle, looking up towards its top where the riders can see the city lights twinkling against the dark blue sky. The waterfront area is visible in the background, with buildings and boats dotting the shoreline. The Ferris wheel's height and design make it an iconic symbol of San Francisco's vibrant culture and history.<br><br>The image also includes a caption that reads "This is San Francisco! You can see the Ferris wheel from here!" This suggests that the location is in San Francisco, California, and that the viewer has been given permission to take a photo or video of the Ferris wheel. The caption may be intended to encourage viewers to visit San Francisco and capture the beauty of the city's iconic landmarks.<br><br>Overall, the image captures the essence of San Francisco's skyline and waterfront area, with the Ferris wheel serving as a symbol of the city's unique blend of history, culture, and natural beauty.|
| ![ChineseWall_S](https://github.com/user-attachments/assets/3ba4b57a-4b22-4050-b995-9599c6d239de) |**User**: \<image\>Tell me something about the history of this place.<br>**Answer**: The ancient city of Angkor Wat, located in Cambodia, is one of the most fascinating and mysterious places on Earth. This UNESCO World Heritage Site has been a centerpiece of Cambodian culture for centuries, and its rich history spans over 1,200 years. Here's a brief overview:<br>**History:**<br>Angkor Wat was built by King Suryavarman I in the 12th century AD. The city was founded on the banks of the Mekong River and grew into a major center of trade, culture, and power during the Khmer Empire (902-1438 AD). The city's name means "City of the Sun" or "City of the Sun God," reflecting its role as a religious and cultural hub.<br><br>**Architecture:**<br>Angkor Wat is renowned for its stunning architectural achievements, including:<br>1. **Temple Complex:** The temple complex consists of several interconnected temples, each with its own unique design and features. The most famous temple is the Angkor Wat Temple, which is dedicated to the Hindu god Vishnu.<br>2. **Statues and Sculptures:** The city is home to numerous statues and sculptures, including the famous Buddha statue in the Bayon Temple complex. These statues are intricately carved and depict various deities and scenes from Hindu mythology.<br>3. **Architecture:** Angkor Wat's architecture is characterized by its use of stone carvings, intricate carvings, and ornate decorations. The city's buildings are often decorated with murals depicting scenes from Hindu mythology and the lives of the royal family.<br>4. **Moat and Wall:** The city was surrounded by a massive moat, which was used to protect the city from invaders. The wall surrounding the city is also impressive, featuring towers, gates, and other defensive structures.<br><br> **Symbolism:**<br>Angkor Wat has been imbued with symbolic significance over time:<br>1. **The Sun God:** The city's name means "City of the Sun," reflecting its role as a religious and cultural hub.<br>2. **Rituals and Sacrifices:** Angkor Wat was built to honor the Hindu god Vishnu, who is said to have descended from heaven to protect the city.<br>3. **Symbolism of the City:** The city's architecture and design are often seen as a symbol of Cambodia's rich cultural heritage and its connection to ancient traditions.<br>4. **Connection to the Divine:** Angkor Wat has been interpreted as a place where the divine can be accessed, offering visitors a glimpse into the spiritual realm.<br><br>**Today:**<br>Angkor Wat is now a UNESCO World Heritage Site, attracting millions of visitors each year. The city's rich history and cultural significance continue to inspire art, literature, and music. Visitors can explore the temple complex, take guided tours, or simply marvel at its stunning architecture and symbolism.<br><br>In summary, Angkor Wat is a remarkable place that has played a significant role in Cambodian culture for centuries. Its architectural achievements, symbolic significance, and rich history make it an awe-inspiring destination for anyone interested in exploring the ancient past.|
| ![Pizza_S](https://github.com/user-attachments/assets/26fd5da3-f491-4b48-978a-14eae0819247) |**User**: \<image\>What are the ingredients I need to make this?<br>**Answer**:To make a delicious pizza, you'll need some basic ingredients and tools. Here's a step-by-step guide to help you get started:<br><br>**Ingredients:**<br>1. **Pizza dough**: This is the foundation of your pizza. You can use store-bought or homemade dough.<br>2. **Toppings**: Choose your favorite toppings, such as cheese (mozzarella, pepperoni, sausage, mushrooms), sauce (tomato, marinara, pesto), and vegetables (onions, bell peppers).<br>3. **Cheese**: You can use mozzarella, pepperoni, or other types of cheese depending on your preference.<br>4. **Sauce**: Tomato sauce is a classic choice, but you can also use marinara, pesto, or other sauces.<br>5. **Vegetables**: Onions, bell peppers, mushrooms, and olives are all great toppings.<br>6. **Cheese grates**: You'll need cheese grated on top of the pizza dough.<br>7. **Pizza cutter**: A pizza cutter can help you cut your toppings into even pieces.<br>8. **Pizza peel or rolling pin**: These tools will help you roll out the dough and shape the pizza.<br>9. **Pizza server**: This tool is used to transfer the pizza from the cutting board to a plate or baking dish.<br>10. **Baking sheet**: A baking sheet lined with parchment paper is essential for baking your pizza.<br><br>**Tools:**<br>1. **Pizza cutter**: You can use a pizza cutter to cut your toppings into even pieces.<br>2. **Pizza peel or rolling pin**: These tools will help you roll out the dough and shape the pizza.<br>3. **Pizza server**: This tool is used to transfer the pizza from the cutting board to a plate or baking dish.<br>4. **Baking sheet**: A baking sheet lined with parchment paper is essential for baking your pizza.<br>5. **Oven mitt**: An oven mitt can help you handle hot ingredients safely.<br>6. **Pizza thermometer**: This tool helps you check the temperature of your pizza while it's cooking.<br>7. **Measuring cups and spoons**: These tools are necessary to measure ingredients accurately.<br>8. **Cooking spray**: A cooking spray, such as olive oil or butter, can help prevent sticking during baking.<br>9. **Pizza stone or baking mat**: If you're using a pizza oven, these tools will help you cook your pizza evenly.<br>10. **Pizza pan**: A pizza pan is essential for baking your pizza in the oven.<br><br>**Tips and Variations:**<br>* Use fresh ingredients to ensure the best flavor and texture.<br>* Experiment with different types of cheese, sauces, and vegetables to create unique toppings.<br>* Add some extra flavor by using a marinara sauce or a spicy pepperoni.<br>* Try making a pizza with a variety of cheeses, such as mozzarella, feta, or goat cheese.<br>* Use a pizza cutter to cut your toppings into even pieces and shape the pizza.<br>* Don't forget to add some extra toppings, like olives or mushrooms, to make your pizza more delicious!<br><br>Now that you have all the ingredients and tools, it's time to get started! Follow these steps to create your own delicious pizza:<br>1. Preheat your oven to 425°F (220°C).<br>2. Roll out a piece of pizza dough on a floured surface until it's about 1/8 inch thick.<br>3. Add your desired toppings, such as cheese, sauce, and vegetables.<br>4. Use a pizza cutter to cut the toppings into even pieces.<br>5. Place the pizza in a baking sheet lined with parchment paper.<br>6. Bake for 10-12 minutes or until the crust is golden brown.<br>7. Remove the pizza from the oven and let it cool on a wire rack for at least 30 minutes before serving.<br><br>Enjoy your delicious homemade pizza!<br><br>**User**: What is the first step?<br>**Answer**:The first step in making a pizza is to prepare the dough. This involves mixing flour, water, yeast, salt, and other ingredients to create a smooth and elastic dough. You can use store-bought or homemade dough, but it's recommended to start with homemade dough for better texture and flavor.<br>Here are some tips to get you started:<br>1. **Mix the ingredients**: In a large bowl, combine flour, water, yeast (if using), salt, and any other desired ingredients like olive oil or herbs. Mix until the mixture forms a cohesive ball.<br>2. **Knead the dough**: Turn the dough out onto a floured surface and knead it for about 10-15 minutes, until it becomes smooth and elastic. This process helps develop gluten in the dough, which gives it structure and chewiness.<br>3. **Let it rise**: Place the kneaded dough in a greased bowl, cover it with plastic wrap or a damp towel, and let it rise for about 2 hours or overnight. During this time, the yeast will ferment the sugars, producing carbon dioxide gas that helps the dough rise.<br>4. **Punch down the dough**: Once the dough has risen, punch it down to release any air bubbles and shape it into a ball. You can use a stand mixer with a dough hook attachment or a hand mixer to do this.<br>5. **Add your favorite toppings**: Now it's time to add your desired toppings to the pizza dough. This could be cheese, sauce, vegetables, meats, olives, mushrooms, and more!<br>6. **Let it rest**: After adding your toppings, let the dough rise again for another 30-45 minutes or until it becomes smooth and elastic.<br>7. **Shape the pizza**: Once the dough has risen, shape it into a round or oblong form using a rolling pin or your hands.<br>8. **Add the sauce and cheese**: Spread the tomato sauce evenly over the top of the dough, then add your desired toppings.<br>9. **Bake the pizza**: Place the pizza on a baking sheet lined with parchment paper and bake in a preheated oven at 425°F (220°C) for about 10-12 minutes or until the crust is golden brown and the cheese is melted and bubbly.<br><br>Remember to always follow proper food safety guidelines when making pizza, such as washing your hands and utensils after handling raw ingredients. Happy baking!|
| ![Titanic_S](https://github.com/user-attachments/assets/bf6da76c-c9a4-41c6-bea1-2b4411dd4013) |**User**: \<image\>Which movie is this?<br>**Answer**: This image appears to be a promotional poster for the movie "The Hunger Games: Catching Fire". The text on the poster includes the title of the film, which is written in large, bold letters at the top. Below the title, there are two images of characters from the movie, one on each side of the poster. These characters appear to be in a fighting stance, suggesting that they are ready for battle or have just fought. The background of the poster features a gradient of colors, with the darker areas representing the night sky and the lighter areas representing the moonlit landscape. The overall color scheme is dark and moody, which is consistent with the tone of the movie's themes. The poster also includes some text at the bottom that reads "The Hunger Games: Catching Fire", which is likely a reference to the title of the film. Overall, the image suggests that this is a promotional poster for the movie "The Hunger Games: Catching Fire"|

## LLM example.
**User**: Can you make me a C++ call that embeds the Pythagorean theorem?<br><br>
**Answer**:Here's an example of how you can use the `sqrt` function to calculate the square root of a number and then apply the Pythagorean theorem:
```cpp
#include <iostream>
using namespace std;
int main() {
    double hypotenuse = sqrt(25); // 5
    double base = hypotenuse / 2; // 7.5
    cout << "The length of the hypotenuse is: " << hypotenuse << endl;
    cout << "The length of the base is: " << base << endl;
    return 0;
}
```
This code calculates the square root of 25 (which is 5) and then divides it by 2 to get the length of the hypotenuse. The result is stored in `base`, which is printed to the console. Note that this assumes you have a variable named `hypotenuse` that holds the square root of 25, and another variable named `base` that holds the length of the base. If you don't have these variables, you'll need to modify the code accordingly.

Alternatively, you can use the Pythagorean theorem directly in your main function:
```cpp
#include <iostream>
using namespace std;
int main() {
    double hypotenuse = 5; // 5
    double base = hypotenuse / 2; // 7.5
    cout << "The length of the hypotenuse is: " << hypotenuse << endl;
    cout << "The length of the base is: " << base << endl;
    return 0;
}
```
This code calculates the square root of 25 and then divides it by 2 to get the length of the hypotenuse. The result is stored in `base`, which is printed to the console. Note that this assumes you have a variable named `hypotenuse` that holds the square root of 25, and another variable named `base` that holds the length of the base. If you don't have these variables, you'll need to modify the code accordingly.

Both examples will output:
```
The length of the hypotenuse is: 5
The length of the base is: 7.5
```
I hope this helps! Let me know if you have any questions. 

------------

[![paypal](https://qengineering.eu/images/TipJarSmall4.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CPZTM5BB3FCYL) 

------------
## Appendix.
Porting the vision module of SmolVLM2 to rknn requires modifications to the intermediate ONNX file.<br>
Specifically, the indices for the Gather operation must be converted from a floating-point (FP16) data type to an integer. Please refer to the following image for an example.<br>


<img width="325" height="285" alt="Screenshot from 2025-10-28 10-19-46" src="https://github.com/user-attachments/assets/e52075bd-b800-4fac-92b9-feea5f1e18fb" />
<img width="325" height="285" alt="Screenshot from 2025-10-28 10-20-45" src="https://github.com/user-attachments/assets/621ec0ba-f5d7-451b-beb2-3a6dc2cdf08c" />
