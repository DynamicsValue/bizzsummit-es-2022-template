---
marp: true
theme: bizzsummit
paginate: true
footer: '![image](./images/BizzSummitLogoBig.png)'
---

<style>
@import 'https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css';
</style>

![bg](./images/BizzSummitBackGroundTitle.png)

<style scoped>
img {
    margin-top: -30px;
    height: 300px;
    margin-left: 360px;
    text-align: center;
}
h1 {
  font-size: 80px;
  color: #fff;
  text-align: center;
  vertical-align: middle;
  margin-top: 5px;
}
h3 {
  font-family: 'Roboto';
  text-align: center;
  vertical-align: middle;
  margin-top: 30px;
}

</style>

![image](./images/BizzSummitLogoBig.png)

# Título Sesión
### Jordi Montaña

---

![bg](./images/Sponsors.svg)

---

![bg](./images/BizzSummitBackGround.png)




<hr/>

# HEADING 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

Normal text 


---
![bg](./images/BizzSummitBackGround.png)

<hr/>

# HEADING 1

Normal text

- Some bullet points
- Some other bullet points

---

![bg](./images/BizzSummitBackGround.png)

# Table format


|Col1|Col2|Col3|
|----|----|----|
|Some value|Another Value| Another one|


---

![bg](./images/BizzSummitBackGround.png)

# Code snippet format 

``` csharp
// Some code here
public class FakeXrmEasyTestsBase
{
    private readonly IXrmFakedContext _context;
    private readonly IOrganizationService _service;

    public FakeXrmEasyTestsBase() 
    {
        _context = MiddlwareBuilder.New();
        _service = _context.GetOrganizationService();
    }
}
``` 

---
![bg](./images/BizzSummitBackGround.png)

# Code snippet (II)

``` csharp
public class FollowupPlugin : IPlugin
{
    //Omitted for simplicity, please see the full source code above
}
```
# Quote styles

Some paragragraph with a `quote text here`. 
>Some quoted text here 

> Another Quote here

    Some indented text here
    Which continues

---
![bg](./images/BizzSummitBackGround.png)

# Other

###### heading 6

<!-- _class: swipe -->
###### Swipe


Some other text

---
![bg](./images/BizzSummitBackGround.png)
# Notes


<!-- Some presenter notes -->

