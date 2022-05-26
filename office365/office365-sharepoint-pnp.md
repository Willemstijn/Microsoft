s# SharePoint Framework and Patterns & practices

## Installation of dev environment for SPF development

Follow instructions for installing Node.js
https://github.com/nodesource/distributions/blob/master/README.md

curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

sudo apt-get install -y nodejs

npm install -g yo gulp


## Techniques
Be familiar with the following development techniques:
* Javascript engine 'Node.js' (afkomstig uit Chrome browser - installeren via instructies website: https://nodejs.org)
* Package manager 'npm' (update met ``npm update -g npm``) - node package manager (SharePoint packages are node projects)
* Package manager 'Yarn' (optional but recommended) - ``npm install -g yarn`` (global installation -g)
* Module bundler for distribution 'Webpack' (www.webpack.js.org)
* Scaffolding engine 'Yeoman' - installation with: ``npm install -g yo gulp``
* Programming language 'Typescript' - Javascript enhancer
* Style sheets with 'Sass' (synthetically awesome style sheets) - CSS improvement
* json
* 
* 

## npm tips
* ``npm list -g --depth=0``  : show top level packages (no dependency packages)
* ``npm init`` : start new project in current folder
* ``npm install`` : na aanpassen package.json met dependencies dit commando uitvoeren om deps te downloaden


## yarn tips


## Notes
"package.json" contains the information of the npm package that you want to distribute. It contains references to other used pakages within your script etc.