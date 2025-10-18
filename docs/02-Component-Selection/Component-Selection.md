---
title: Component Selection
---


**Push Button**

1. TS02-66-50-BK-100-LCR-D -- Push Button

    ![](pushbtnimg.webp)

    * $0.1/each
    * [link to product](https://www.digikey.com/en/products/detail/same-sky-formerly-cui-devices-/TS02-66-50-BK-100-LCR-D/15634294?gclsrc=aw.ds&gad_source=1&gad_campaignid=20243136172&gbraid=0AAAAADrbLlhjMd1SI_TeFQt_5_XtjL5xo&gclid=CjwKCAjw0sfHBhB6EiwAQtv5qTL9B8Ibio6YzZwawgjQTTJfZOm_s9jRh1qKtTfUlaCdgvFL-coORxoCgQgQAvD_BwE)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | Cheap                               | No Dust Protection |
    | Good Operating Temperature                      | No Water Protection                                        |
    | Meets surface mount constraint of project |

2. C&K D6 Series Keyswitch

    ![](docs\02-Component-Selection\pshbtn2.jpg)

    * $1.6/each
    * [Link to product](https://www.digikey.com/en/products/detail/c-k/D6R10-F2-LFS/1466347)

    | Pros                                                              | Cons                |
    | ----------------------------------------------------------------- | ------------------- |
    | Cheap                                                             | More expensive      |
    | Good Operating Temperature                                 | Not Water Resistant|
    |More Tactile/Better Quality|

3. SW-PB11-4CSKG-10 Push Button
 
    ![](pshbtn3.jpg)

    * $6.43/each
    * [Link to product](https://www.digikey.com/en/products/detail/adam-tech/SW-PB11-4CSKG-10/14635466?utm_source=Perplexity&utm_medium=referral)

    | Pros                                                              | Cons                |
    | ----------------------------------------------------------------- | ------------------- |
    | Dust Proof                                                             | Expensive      |
    | Water Proof                                 
    |Heavy Duty|



**Choice:** Option 2: C&K D6 Series Keyswitch   

**Rationale:** Since adjacent components in the product also require protection, we must use an enclosure for the entire assembly. This makes the IP67 rating of Option 3 redundant. By using a single enclosure, we can use the higher-quality C&K switch, which offers superior tactile feedback and a much longer 250,000-cycle life, providing better reliability.




**Pull Down Resistor**

1. 10K Ohm SMD Resistor

    ![](smdresist.webp)

    * $0.1/each
    * [link to product](https://www.digikey.com/en/products/detail/same-sky-formerly-cui-devices-/TS02-66-50-BK-100-LCR-D/15634294?gclsrc=aw.ds&gad_source=1&gad_campaignid=20243136172&gbraid=0AAAAADrbLlhjMd1SI_TeFQt_5_XtjL5xo&gclid=CjwKCAjw0sfHBhB6EiwAQtv5qTL9B8Ibio6YzZwawgjQTTJfZOm_s9jRh1qKtTfUlaCdgvFL-coORxoCgQgQAvD_BwEhttps://www.digikey.com/en/products/detail/yageo/RC0603FR-0710KL/726880)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | Smaller Form Factor                               | Difficult to Hand Solder |
    | Moisture Resistant                 
    | Meets surface mount constraint of project |

2. 10K Ohm SMD Resistor

    ![](thtresist.webp)

    * $0.1/each
    * [Link to product](https://www.digikey.com/en/products/detail/stackpole-electronics-inc/CF14JA10K0/21720224?gclsrc=aw.ds&gad_source=1&gad_campaignid=20682878391&gbraid=0AAAAADrbLlgbcuo_fYsHwsmmzciY1a4iV&gclid=CjwKCAjw0sfHBhB6EiwAQtv5qe8kMZm3tYG9zKv_h9EisWxUUe3a8mT-ah0_GFUkHc7hP42Tdb7hGRoCVeYQAvD_BwE)

    | Pros                                                              | Cons                |
    | ----------------------------------------------------------------- | ------------------- |
    | Easy to Solder & Prototype                                                             |Large   |
    | Good Operating Temperature                                 
    |Easy to Repair/Replace|



**Choice:** Option 2: 10K Ohm SMD Resistor

**Rationale:** We're using a THT resistor. For a simple RC debounce filter, the miniaturization and high-frequency benefits of SMD are unnecessary, while the durability and ease of soldering of a THT resistor are major advantages for reliability and servicing.


**LED**

1. LTST-C170GKT - SMD LED

    ![](smdlked.webp)

    * $0.1/each
    * [link to product](https://www.digikey.com/en/products/detail/same-sky-formerly-cui-devices-/TS02-66-50-BK-100-LCR-D/15634294?gclsrc=aw.ds&gad_source=1&gad_campaignid=20243136172&gbraid=0AAAAADrbLlhjMd1SI_TeFQt_5_XtjL5xo&gclid=CjwKCAjw0sfHBhB6EiwAQtv5qTL9B8Ibio6YzZwawgjQTTJfZOm_s9jRh1qKtTfUlaCdgvFL-coORxoCgQgQAvD_BwE)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | Cheap                               | Extremely Small |
    | Wide Viewing Angle                       | SMD                                        |

2. 151051RS11000 - THT LED

    ![](tntled.webp)

    * $0.15/each
    * [Link to product](https://www.digikey.com/en/products/detail/w-rth-elektronik/151051RS11000/4490012?gclsrc=aw.ds&gad_source=1&gad_campaignid=20228387720&gbraid=0AAAAADrbLlgDBce-yW0NxfO-Ob4qZ3vYc&gclid=CjwKCAjw0sfHBhB6EiwAQtv5qefY9cZibBmWByq1iOIOMS7wTNOeG6vDpariQ8GL5ejDJzHyzCcToRoCz6sQAvD_BwE)

    | Pros                                                              | Cons                |
    | ----------------------------------------------------------------- | ------------------- |
    | THT                                                             | More expensive      |
    | Highly Visible                            | Fragile|

3. L65DR12L - Pilot Panel Light
 
    ![](panellight.webp)

    * $3.75/each
    * [Link to product](https://www.digikey.com/en/products/detail/visual-communications-company-vcc/L65DR12L/6166300?gclsrc=aw.ds&gad_source=1&gad_campaignid=20228387720&gbraid=0AAAAADrbLlgDBce-yW0NxfO-Ob4qZ3vYc&gclid=CjwKCAjw0sfHBhB6EiwAQtv5qR5vGshBwh-8JN3RyWXDJwwFqjLhzi-i4aZ8F2Z6iBGFRKPGzl7QPBoCdM0QAvD_BwE)

    | Pros                                                              | Cons                |
    | ----------------------------------------------------------------- | ------------------- |
    | Dust Proof                                                             | Expensive      |
    | Water Proof                                 
    |Heavy Duty|



**Choice:** Option 2: 51051RS11000 - THT LED

**Rationale:** We're selecting Option 2 (THT LED). It avoids the difficult manual soldering of the SMD (Option 1) and the unnecessary cost and complexity of the panel-mount (Option 3). Option 2 is the simplest, most cost-effective choice for our manual assembly process.


