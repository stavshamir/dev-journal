## Monday 21/01/19

---

##### Problem: Serving an Angular app from Boot Spring
Suggested Solution:
1. Build the angular app:  
    ```sh
    $ ng build --prod
    ```
2. In the Spring Boot project, create a new directory named 'public' under 'resources'.
3. Copy the files under 'dist/`<project-name>`' from the angular project to 'resources/public'
4. The app should be accessible from `<host-ip>`:8080
5. If the root html file is names something other than index.html, it will be availale at `<host-ip>`:8080/`<filename>`.html
    
*Tags: angular, boot spring*

---


## Tuesday 16/01/19

---

##### Problem: Creating a custom CSS class using Bootstrap classes
Suggested Solution:
1. Add [Bootstrap classes definitions](https://maxcdn.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.css) to custom class 

*Tags: css, bootstrap*

---

##### Problem: Angular development server does is not accessible
Suggested Solution:
1. Open `<project-root>`/package.json
2. Under scripts/start, the value is "ng serve" - change it to "ng serve --host 0.0.0.0"

*Tags: angular*

## Tuesday 15/01/19

---

##### Problem: Adding Bootstrap to an Angular project
Suggested Solution:
1. Go to the project's directory
2. Run:
    ```sh
    $ npm install bootstrap@4 --save
    ```
3. Go to `<project-root>`/node_modules/bootstrap/dist/css and copy the relative path of bootstrap.css
4. In angular.json (also in project root), under style, paste the path

*Tags: angular, bootstrap*

