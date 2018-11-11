**How ReactJS shows images based on different conditions** 

Syntax: { condition ? 'text1' : 'text2'}  / 

​             {condition ? ( <img src={ } alt="" />) : 'text'} /

​             {condition ? ( <img src={ } alt=" " /> ) : ( <img src={ } alt=" " />)}

**need to declare on top that where the image is imported from

```
import React, { Component } from "react";
import DEFAULT_IMAGE from "/Users/andreakim/Documents/Project_2Wk/client-sunny/src/Components/Headline/defaultIMG_08.jpg";

<div className="card-body">

              <div>

                {article.image_url ? (

                  <img src={article.image_url} alt="" />

                ) : (

                  <img src={DEFAULT_IMAGE} alt="" />

                )}

              </div>

              <span className="text-muted">#{article.rank}</span>

```

```
render () {
  const { item, i } = this.props ;
    return (
      <div className="grid-box">
        {this.state.hover ? (
           <img src={Eyecon} /> 
        ) : null }
      </div>
    )
}
```

