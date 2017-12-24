# ihepta.github.io-config

include personal config and source files of ihepta.github.io

### How to use theme tranquilpeak build your blog , let's mark some key steps：

1. Use hexo to init your blog folder and install dependencies

2. Download the tranquilpeak source code and do some initial work

   1. Run `git clone https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak.git`

   2. Rename the folder in `tranquilpeak` and place it in `themes` folder of your Hexo blog

   3. Modify the theme in `_config.yml` by changing `theme` variable to `tranquilpeak`

   4. Complete `theme/tranquilpeak/_config.yml` with your information by following directives in comments

   5. Go in `theme/tranquilpeak` folder with `cd themes/tranquilpeak`

   6. Install [requirements](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak/blob/master/docs/developer.md#requirements)

   7. Run `npm install` to install [NPM dependencies](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak/blob/master/docs/developer.md#npm-dependencies)

   8. Run `bower install` to install [Bower dependencies](https://github.com/LouisBarranqueiro/hexo-theme-tranquilpeak/blob/master/docs/developer.md#bower-dependencies)

   9. Modify `/themes/tranquilpeak/source/_css/utils/_variables.scss`  row 218

      `$main-content: {`

      ​	`'max-width':          750px,`

      ​     ` 'padding-right-left': 20px`

      `}` from `750px` to `80%`

   10. Biography and job is editable in each languages files in `/themes/tranquilpeak/languages` folder

   11. Copy your source images to `/themes/tranquilpeak/source/_images` folder

   12. Copy your scaffolds folder to `/themes/tranquilpeak/scaffolds` folder

   13. Run `npm run prod` to build the themes

3. Go in your blog folder and modify `node_modules/hexo-generator-index/lib/generator.js` to make the posts sort by top value

4. When you want to deploy your blog , you need to run `npm install hexo-deployer-git --save` first

5. Finally, Run `hexo g -d`  Everything have done ,cool !  :)

