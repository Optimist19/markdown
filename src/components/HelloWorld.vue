<template>
  <div class="hello">
    <!-- perfect markdown example -->
    <div v-html="renderedContent"></div>

   
    <textarea v-model="content"></textarea>
  </div>
</template>

<script>
import MarkdownIt from 'markdown-it';
import {ref, computed} from "vue"
import DOMPurify from 'dompurify';


export default {
  name: 'HelloWorld',
  setup(){
    const content = ref('')

    // Additionally, you can enhance the editor experience by providing toolbar buttons or shortcuts for common formatting options. You can use existing libraries like v-markdown-editor or create your own custom toolbar components to simplify the formatting process for users.

    const md = new MarkdownIt({
      html: true,
      breaks: true,
      linkify: true,
    })

      const renderedContent = computed(() => {
      const sanitiseHtml  = md.render(content.value);
      // The content is the user input which the md.render method converts the Markdown syntax eg ## to html tag which is an html header(h2). If you want to be sure, do not put the renderedContent in "v-html" in an opening tag, instead like a mustache "{{ }}"
      return DOMPurify.sanitize(sanitiseHtml);
    });


    return{
      md,
      content,
      renderedContent
    }
  }

// 1. Do 'npm install markdown-it' to install the Markdown parser and then export it in the script tag.

// 2. You will do this, here's an example of creating a Markdown instance with some basic configuration options: 
//     const md = new MarkdownIt({
//       html: true,
//       breaks: true,
//       linkify: true,
//     })

// After creating the Markdown instance, you can use the md.render() method to parse Markdown content into HTML.

// 3. The MarkdownIt constructor accepts an optional configuration object where you can specify various options for the Markdown parser. Here are some common configuration options:

// * html (default: false): Set it to true to enable HTML tags in the input.
// * breaks (default: false): Set it to true to convert newlines into <br> tags.
// * linkify (default: false): Set it to true to automatically convert URLs into links.

// 4. Install DOMPurify from dompurify; 
// The purpose of the DOMPurify is to ensure that any potentially unsafe elements or attributes are removed or neutralized, reducing the risk of cross-site scripting (XSS) attacks or other security vulnerabilities.

// The link below is a github repo that shows some markdown syntax which you can use to text this markdown

// https://markdown-it.github.io/#md3=%7B%22source%22%3A%22---%5Cn__Advertisement%20%3A%29__%5Cn%5Cn-%20__%5Bpica%5D%28https%3A%2F%2Fnodeca.github.io%2Fpica%2Fdemo%2F%29__%20-%20high%20quality%20and%20fast%20image%5Cn%20%20resize%20in%20browser.%5Cn-%20__%5Bbabelfish%5D%28https%3A%2F%2Fgithub.com%2Fnodeca%2Fbabelfish%2F%29__%20-%20developer%20friendly%5Cn%20%20i18n%20with%20plurals%20support%20and%20easy%20syntax.%5Cn%5CnYou%20will%20like%20those%20projects!%5Cn%5Cn---%5Cn%5Cn%23%20h1%20Heading%208-%29%5Cn%23%23%20h2%20Heading%5Cn%23%23%23%20h3%20Heading%5Cn%23%23%23%23%20h4%20Heading%5Cn%23%23%23%23%23%20h5%20Heading%5Cn%23%23%23%23%23%23%20h6%20Heading%5Cn%5Cn%5Cn%23%23%20Horizontal%20Rules%5Cn%5Cn___%5Cn%5Cn---%5Cn%5Cn%2A%2A%2A%5Cn%5Cn%5Cn%23%23%20Typographic%20replacements%5Cn%5CnEnable%20typographer%20option%20to%20see%20result.%5Cn%5Cn%28c%29%20%28C%29%20%28r%29%20%28R%29%20%28tm%29%20%28TM%29%20%28p%29%20%28P%29%20%2B-%5Cn%5Cntest..%20test...%20test.....%20test%3F.....%20test!....%5Cn%5Cn!!!!!!%20%3F%3F%3F%3F%20%2C%2C%20%20--%20---%5Cn%5Cn%5C%22Smartypants%2C%20double%20quotes%5C%22%20and%20%27single%20quotes%27%5Cn%5Cn%5Cn%23%23%20Emphasis%5Cn%5Cn%2A%2AThis%20is%20bold%20text%2A%2A%5Cn%5Cn__This%20is%20bold%20text__%5Cn%5Cn%2AThis%20is%20italic%20text%2A%5Cn%5Cn_This%20is%20italic%20text_%5Cn%5Cn~~Strikethrough~~%5Cn%5Cn%5Cn%23%23%20Blockquotes%5Cn%5Cn%5Cn%3E%20Blockquotes%20can%20also%20be%20nested...%5Cn%3E%3E%20...by%20using%20additional%20greater-than%20signs%20right%20next%20to%20each%20other...%5Cn%3E%20%3E%20%3E%20...or%20with%20spaces%20between%20arrows.%5Cn%5Cn%5Cn%23%23%20Lists%5Cn%5CnUnordered%5Cn%5Cn%2B%20Create%20a%20list%20by%20starting%20a%20line%20with%20%60%2B%60%2C%20%60-%60%2C%20or%20%60%2A%60%5Cn%2B%20Sub-lists%20are%20made%20by%20indenting%202%20spaces%3A%5Cn%20%20-%20Marker%20character%20change%20forces%20new%20list%20start%3A%5Cn%20%20%20%20%2A%20Ac%20tristique%20libero%20volutpat%20at%5Cn%20%20%20%20%2B%20Facilisis%20in%20pretium%20nisl%20aliquet%5Cn%20%20%20%20-%20Nulla%20volutpat%20aliquam%20velit%5Cn%2B%20Very%20easy!%5Cn%5CnOrdered%5Cn%5Cn1.%20Lorem%20ipsum%20dolor%20sit%20amet%5Cn2.%20Consectetur%20adipiscing%20elit%5Cn3.%20Integer%20molestie%20lorem%20at%20massa%5Cn%5Cn%5Cn1.%20You%20can%20use%20sequential%20numbers...%5Cn1.%20...or%20keep%20all%20the%20numbers%20as%20%601.%60%5Cn%5CnStart%20numbering%20with%20offset%3A%5Cn%5Cn57.%20foo%5Cn1.%20bar%5Cn%5Cn%5Cn%23%23%20Code%5Cn%5CnInline%20%60code%60%5Cn%5CnIndented%20code%5Cn%5Cn%20%20%20%20%2F%2F%20Some%20comments%5Cn%20%20%20%20line%201%20of%20code%5Cn%20%20%20%20line%202%20of%20code%5Cn%20%20%20%20line%203%20of%20code%5Cn%5Cn%5CnBlock%20code%20%5C%22fences%5C%22%5Cn%5Cn%60%60%60%5CnSample%20text%20here...%5Cn%60%60%60%5Cn%5CnSyntax%20highlighting%5Cn%5Cn%60%60%60%20js%5Cnvar%20foo%20%3D%20function%20%28bar%29%20%7B%5Cn%20%20return%20bar%2B%2B%3B%5Cn%7D%3B%5Cn%5Cnconsole.log%28foo%285%29%29%3B%5Cn%60%60%60%5Cn%5Cn%23%23%20Tables%5Cn%5Cn%7C%20Option%20%7C%20Description%20%7C%5Cn%7C%20------%20%7C%20-----------%20%7C%5Cn%7C%20data%20%20%20%7C%20path%20to%20data%20files%20to%20supply%20the%20data%20that%20will%20be%20passed%20into%20templates.%20%7C%5Cn%7C%20engine%20%7C%20engine%20to%20be%20used%20for%20processing%20templates.%20Handlebars%20is%20the%20default.%20%7C%5Cn%7C%20ext%20%20%20%20%7C%20extension%20to%20be%20used%20for%20dest%20files.%20%7C%5Cn%5CnRight%20aligned%20columns%5Cn%5Cn%7C%20Option%20%7C%20Description%20%7C%5Cn%7C%20------%3A%7C%20-----------%3A%7C%5Cn%7C%20data%20%20%20%7C%20path%20to%20data%20files%20to%20supply%20the%20data%20that%20will%20be%20passed%20into%20templates.%20%7C%5Cn%7C%20engine%20%7C%20engine%20to%20be%20used%20for%20processing%20templates.%20Handlebars%20is%20the%20default.%20%7C%5Cn%7C%20ext%20%20%20%20%7C%20extension%20to%20be%20used%20for%20dest%20files.%20%7C%5Cn%5Cn%5Cn%23%23%20Links%5Cn%5Cn%5Blink%20text%5D%28http%3A%2F%2Fdev.nodeca.com%29%5Cn%5Cn%5Blink%20with%20title%5D%28http%3A%2F%2Fnodeca.github.io%2Fpica%2Fdemo%2F%20%5C%22title%20text!%5C%22%29%5Cn%5CnAutoconverted%20link%20https%3A%2F%2Fgithub.com%2Fnodeca%2Fpica%20%28enable%20linkify%20to%20see%29%5Cn%5Cn%5Cn%23%23%20Images%5Cn%5Cn!%5BMinion%5D%28https%3A%2F%2Foctodex.github.com%2Fimages%2Fminion.png%29%5Cn!%5BStormtroopocat%5D%28https%3A%2F%2Foctodex.github.com%2Fimages%2Fstormtroopocat.jpg%20%5C%22The%20Stormtroopocat%5C%22%29%5Cn%5CnLike%20links%2C%20Images%20also%20have%20a%20footnote%20style%20syntax%5Cn%5Cn!%5BAlt%20text%5D%5Bid%5D%5Cn%5CnWith%20a%20reference%20later%20in%20the%20document%20defining%20the%20URL%20location%3A%5Cn%5Cn%5Bid%5D%3A%20https%3A%2F%2Foctodex.github.com%2Fimages%2Fdojocat.jpg%20%20%5C%22The%20Dojocat%5C%22%5Cn%5Cn%5Cn%23%23%20Plugins%5Cn%5CnThe%20killer%20feature%20of%20%60markdown-it%60%20is%20very%20effective%20support%20of%5Cn%5Bsyntax%20plugins%5D%28https%3A%2F%2Fwww.npmjs.org%2Fbrowse%2Fkeyword%2Fmarkdown-it-plugin%29.%5Cn%5Cn%5Cn%23%23%23%20%5BEmojies%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-emoji%29%5Cn%5Cn%3E%20Classic%20markup%3A%20%3Awink%3A%20%3Acrush%3A%20%3Acry%3A%20%3Atear%3A%20%3Alaughing%3A%20%3Ayum%3A%5Cn%3E%5Cn%3E%20Shortcuts%20%28emoticons%29%3A%20%3A-%29%20%3A-%28%208-%29%20%3B%29%5Cn%5Cnsee%20%5Bhow%20to%20change%20output%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-emoji%23change-output%29%20with%20twemoji.%5Cn%5Cn%5Cn%23%23%23%20%5BSubscript%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-sub%29%20%2F%20%5BSuperscript%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-sup%29%5Cn%5Cn-%2019%5Eth%5E%5Cn-%20H~2~O%5Cn%5Cn%5Cn%23%23%23%20%5B%5C%5C%3Cins%3E%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-ins%29%5Cn%5Cn%2B%2BInserted%20text%2B%2B%5Cn%5Cn%5Cn%23%23%23%20%5B%5C%5C%3Cmark%3E%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-mark%29%5Cn%5Cn%3D%3DMarked%20text%3D%3D%5Cn%5Cn%5Cn%23%23%23%20%5BFootnotes%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-footnote%29%5Cn%5CnFootnote%201%20link%5B%5Efirst%5D.%5Cn%5CnFootnote%202%20link%5B%5Esecond%5D.%5Cn%5CnInline%20footnote%5E%5BText%20of%20inline%20footnote%5D%20definition.%5Cn%5CnDuplicated%20footnote%20reference%5B%5Esecond%5D.%5Cn%5Cn%5B%5Efirst%5D%3A%20Footnote%20%2A%2Acan%20have%20markup%2A%2A%5Cn%5Cn%20%20%20%20and%20multiple%20paragraphs.%5Cn%5Cn%5B%5Esecond%5D%3A%20Footnote%20text.%5Cn%5Cn%5Cn%23%23%23%20%5BDefinition%20lists%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-deflist%29%5Cn%5CnTerm%201%5Cn%5Cn%3A%20%20%20Definition%201%5Cnwith%20lazy%20continuation.%5Cn%5CnTerm%202%20with%20%2Ainline%20markup%2A%5Cn%5Cn%3A%20%20%20Definition%202%5Cn%5Cn%20%20%20%20%20%20%20%20%7B%20some%20code%2C%20part%20of%20Definition%202%20%7D%5Cn%5Cn%20%20%20%20Third%20paragraph%20of%20definition%202.%5Cn%5Cn_Compact%20style%3A_%5Cn%5CnTerm%201%5Cn%20%20~%20Definition%201%5Cn%5CnTerm%202%5Cn%20%20~%20Definition%202a%5Cn%20%20~%20Definition%202b%5Cn%5Cn%5Cn%23%23%23%20%5BAbbreviations%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-abbr%29%5Cn%5CnThis%20is%20HTML%20abbreviation%20example.%5Cn%5CnIt%20converts%20%5C%22HTML%5C%22%2C%20but%20keep%20intact%20partial%20entries%20like%20%5C%22xxxHTMLyyy%5C%22%20and%20so%20on.%5Cn%5Cn%2A%5BHTML%5D%3A%20Hyper%20Text%20Markup%20Language%5Cn%5Cn%23%23%23%20%5BCustom%20containers%5D%28https%3A%2F%2Fgithub.com%2Fmarkdown-it%2Fmarkdown-it-container%29%5Cn%5Cn%3A%3A%3A%20warning%5Cn%2Ahere%20be%20dragons%2A%5Cn%3A%3A%3A%5Cn%22%2C%22defaults%22%3A%7B%22html%22%3Afalse%2C%22xhtmlOut%22%3Afalse%2C%22breaks%22%3Afalse%2C%22langPrefix%22%3A%22language-%22%2C%22linkify%22%3Atrue%2C%22typographer%22%3Atrue%2C%22_highlight%22%3Atrue%2C%22_strict%22%3Afalse%2C%22_view%22%3A%22html%22%7D%7D


}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
