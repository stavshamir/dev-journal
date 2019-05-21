## Tuesday 21/05/19
##### Problem: Search for string in files in a directory
Suggested Solution:
* Recursive grep: `grep -rl string .`

*Tags: search, grep*

---

## Wednsday 03/04/19
##### Problem: Debugging bash scripts
Suggested Solution:
* Run `$ set -x` to get scripts to print the results of inner statement
* Run `$ set +x` to turn it off

*Tags: bash, script, debug*

---

## Wednsday 20/03/19
##### Problem: How to easily create a CLI in python?
Suggested Solution:
[Python Fire](https://github.com/google/python-fire)

Suggested Solution:
[click](https://click.palletsprojects.com)

*Tags: python, cli*

---

## Tuesday 19/03/19
##### Problem: How to print only the nth column of a file?
Suggested Solution:
`$ <cmd> | awk '{print $<n>}'`

Example: `docker service ls | tail -n +2 | awk '{print $2}'`

*Tags: awk, column*

---

## Tuesday 12/03/19
##### Problem: Which ports are taken by which processes?
Suggested Solution:
1. [Install netstat in CentOS](https://cyruslab.net/2014/07/11/installing-netstat-on-centos-7-minimal-installation/).
2. Run `$ netstat -plnt`

*Tags: netstat, ports*

---

## Monday 11/03/19
##### Problem: Providing autocompletion to bash scripts
Suggested Solution:
Create and source the following script:
```bash
_options=$(compgen -W "opt1 opt2 opt3")
complete -o nospace -W "${_options}" 'name-of-function-for-autocomplete'
```
There is more information for more complex autocomplete in [here](https://debian-administration.org/article/316/An_introduction_to_bash_completion_part_1).

*Tags: autocomplete, bash, scripts*

---

## Monday 18/02/19

##### Problem: Configuring a custom authorization method for `@PreAuthorize`
Suggested Solution:
1. Create a bean (let's say `SomeServiceImpl`) with a method returning a boolean
2. Use it like this:
    ```java
    @PreAuthorize("@someServiceImpl.boolMethod()")
    ```
    Notice the syntax of `@<camelCaseBeanClassName>.<method>()`.
3. If needed, an argument of the annotated method can be referenced in the annotation:
    ```java
    @PreAuthorize("@someServiceImpl.boolMethod(#param)")
    public void foo(String param) {
        ...
    }
    ```
Further read:
[Custom authorization method](https://dreamix.eu/blog/java/implementing-custom-authorization-function-for-springs-pre-and-post-annotations)

*Tags: spring boot, @PreAuthorize, custom authorization*

---

##### Problem: Should default values be used in the `topics` attribute of `@KafkaListener`?
Suggested Solution:
```java
@KafkaListener(topics = "${kafka.topic.foo}", containerFactory = "fooFactory")
```
* Cons for using default value - if the value is not found, the default which will be used might be the wrong topic, and in that case system will not
function as expected without any notice. It will be much easier to find the error if it breaks non-silently.
* Pros for using default value - the service will not crash if the value is not found, and if a sensible default is provided things might still
work correctly. 
    
*Tags: spring, @KafkaListener, default*

---

## Tuesday 12/02/19

##### Problem: Tagging a release in a git repository
Suggested Solution:
Tag the last commit when a version is ready to be released with the new version number:
```sh
$ git tag -a vX.Y.Z -F release-notes-filename
```
To push the tags:
```sh
$ git push --tags
```

[Read more](http://alblue.bandlem.com/2011/04/git-tip-of-week-tags.html).
    
*Tags: git, tag, version*

---

## Sunday 10/02/19

##### Problem: Remove a specific line from a file in linux
Suggested Solution:
```sh
$ sed -i '/substring of line to delete/d' filename
```
[More info in SO](https://stackoverflow.com/questions/5410757/delete-lines-in-a-text-file-that-contain-a-specific-string).
    
*Tags: commandline, sed, script*

---

## Wednsday 30/01/19

##### Problem: Add sonatype snapshot dependency in gradle
Suggested Solution:
[This SO answer](https://stackoverflow.com/a/48459437) describes how to do it:
```groovy
repositories {
    mavenCentral()
    // add sonatype repository
    maven {
        url 'https://oss.sonatype.org/content/repositories/snapshots/'
    }
}
```
    
*Tags: gradle, sonatype*

---

## Wednsday 23/01/19

##### Problem: Trying a regular expressions
Suggested Solution:
[This](https://regex101.com/) is a nice tool to quickly and easily test out regular expressions.

Suggested Solution:
[Or This](https://regexr.com/) for interactivity, cheat sheet, and other helpful regexp tools.
    
*Tags: regex, regular expressions*

---

## Tuesday 22/01/19

##### Problem: Creating a jar for an Angular app with gradle
Suggested Solution:
[This link](https://ordina-jworks.github.io/architecture/2018/10/12/spring-boot-angular-gradle.html) provides nice explanation on how to do it.
Also there is an example in my swagger4kafka-ui repository.
    
*Tags: angular, boot spring, jar, gradle*

---

##### Problem: Serving an Angular app from Boot Spring as an external dependency (jar)
Suggested Solution:
1. Build the angular app:  
    ```sh
    $ ng build --prod
    ```
2. Move the results from `dist` to the following directory structure `META-INF/resources`.
3. Create a jar: 
    ```sh
    $ jar -cf <name>.jar META-INF
    ```
4. Add the dependency to the build.gradle of the target Boot Spring project:
    ```groovy
    compile files('full-path-to-jar/name.jar')
    ```
5. The app should be accessible from `<host-ip>:8080`

If there are http requests to the backend, use only the endpoint URL, i.e. change the request url from `http://localhost:8080/endpoint` to `/endpoint`.
    
*Tags: angular, boot spring, jar*

---

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

