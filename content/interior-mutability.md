+++
title = "Interior Mutability in Rust"
description = "How to safely escape the borrow checker"
date = 2023-03-22
tags = ["rust"]
draft = true
+++



Testing some diagrams in the blog post

{% diagram() %}
journey
    title My working day
    section Go to work
      Make tea: 5: Me
      Go upstairs: 3: Me
      Do work: 1: Me, Cat
    section Go home
      Go downstairs: 5: Me
      Sit down: 5: Me
{% end %}

Let's put some text here in between.

{% diagram() %}
journey
    title My working day
    section Go to work
      Make tea: 5: Me
      Go upstairs: 3: Me
      Do work: 1: Me, Cat
    section Go home
      Go downstairs: 5: Me
      Sit down: 5: Me
{% end %}
