---
title: "Beginner's Guide to SystemVerilog & UVM"
date: 2020-09-15T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["SystemVerilog"]
author: "Harish Kumaran"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Desc Text."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: false # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/<path_to_repo>/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

## What is UVM?

UVM (Universal Verification Methodology) is a standardized verification methodology for SystemVerilog.

## Key Components of UVM

- **Testbench**: The environment for verification.
    
- **Sequence & Sequences**: Defines stimulus.
    
- **Driver**: Sends transactions to DUT.
    
- **Monitor**: Captures and analyzes signals.
    

### Example: Basic UVM Sequence

```
class my_sequence extends uvm_sequence;
  `uvm_object_utils(my_sequence)
  task body;
    `uvm_info(get_type_name(), "Running sequence", UVM_MEDIUM)
  endtask
endclass
```

## Why Use UVM?

- Industry standard
    
- Reusability & scalability
    

## Conclusion

If you're into VLSI, learning UVM is a must!