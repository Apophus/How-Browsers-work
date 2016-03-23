   # How Browsers Work.
   
   Browsers are widely used application software and what they do is basically is to request resources chosen by a user from the server and displaying it in the browser window. Usually the resource is a html document but also can be a pdf file, doc, image, mp3 etc. The location of these resources is specified bu a URI(Uniform Resource Identifier). 
   The way the browser interprets and displays HTML files is specified in the HTML and CSS specifications. Browser user interfaces have a lot in common with each other. Among the common user interface elements are:
   *Address bar for inserting a URI
   *Back and forward buttons.
   *Bookmarking options.
   *Refresh and stop buttons for refreshing or stopping the loading of current documents.
   *Home button that takes you to the home page.

 ## The browser's high level structure.
    The browser's main  components are:
  1. The user interface:Includes the address bar, back/forward button bookmarking menu, basically all parts of the browser's display except the window whre you see the requested page.
  2. The browser engine: marshals actions between the UI and the rendering engine.
  3. The rendering engine: responsible for displaying requested content.The rendering engine, for example when a HTML file is requested, parses the HTML and CSS and displays the parsed information on the screen.
  4. Networking:for network calls such as the http requests, using different implementations for different platform behind a platform-independent interface.
  5. UI backend: user for drawing basic widgets like combo boxes and windows.It exposes a generic interfacethat is not platform specific.Underneath it uses operating system user iterface methods.
  6. Data storage: The browser may neeed to store all sorts of data like cookies locally.Browsers also support storage mechanisms such as localStorage, indexedDB, WebSQL and FileSystem.

  ## The rendering engine
  The rendering engine displays the requested content on the screen.By default the rendering engine displays html documents and XML elements and images.It can also display other elements via plug-ins and extensions for example using a PDF viewer plug-in to open pdf files. Different browsers use diffrent rendering engines. Firefox uses Gecko, Safari uses Webkit. Chrome and Opera use Blink, a fork for Webkit.
  ##The main flow.
  The rendering engine will start getting the requested content from the networking layer in 8kB chunks.
  The rendering engine will start parsing the HTML document and convert elements to a DOM node called the **content tree** . Styling information together with visual instructions in the HTML will be used to create another tree called the **rendering tree** .
The render tree contains rectangles with visual attributes like color and dimensions. The rectangles are in the right order to be displayed on the screen.
  After the construction of the render tree, it goes through a layout process. This means giving the codes the extra coordinates where it should appear on the screen.The next stage is painting where the render tree will be traversed and each node will be be painted using the UI backend layer.
For better user experience, the rendering engine will try to display contents on the screen as soon as possible. It does not wait until all HTML is parsed before starting to build and layout the rendered tree. Parts of the contents is parsed and displayed while the process continues with the rest of the contents that keeps kcoming from the network.

  ## Parsing general.
  Parsing a dociment means translating it to a structure the code can use.The result of parsing is a tree of nodes that present the structure of the document.It is called a parse tree or a syntax tree.

  ## Grammars.
  Parsing is based on the syntax rules the document obeys that is the language or format it was written in. Every format you can parse must have deterministic grammmar consisting of vocubulary and syntax rules. It is called a **context free grammar**. 
Human languages are not such languages and therefore cannot be parsed with conventional parsing techniques.

  ## Parser-Lexer combination.
  Parsing can be separated into two sub-groups:
  *Lexical analysis: this is the processs of breaking the input into tokens. TRokens are the language vocubulary: the collection building blocks. In human language it will consist of all the words that appear in the dictionary.
  * Syntax analysis : this is the applying of the language syntax rules. 

  Parsers usually devide the work between two components: the **lexer**(sometimes called the tokenizer) thais responsible for breaking input into valid tokens and the **parser** that constructs the parse tree by analyzing the document structure according to the language syntax rules. The lexer knows how to strip irrelevant characters like white spaces and line breaks. 
   The parsing process is itterative. THe parser will usually ask the lexer for a new token and try to match the token with one of the syntax rules. If a rule is matched a node corresponding to that token will be added to the parse tree and the parser will ask for another token.
   If no rule matches, the parser will store the token internally, and keep asking for tokens until a rule matching all the internally stored tokens is found. If no rule is found then the parser will raise an exception. This means the document was not valid and contained syntax errors.

   ## Translation.
  In many cases the parse tree is not the final product. Parsing is often used in translation: transforming the input document into another format.An example is compilation which compiles a source code into a machine code  first then parses it into a parse tree  and then translates the tree into a machine code document.

  ## Types of parsers.
  THere are two types of parsers.
  *Top down parsers: They examine high level structure of the syntax and try to find a rule match.
  *Bottom up parsers : they start with the input and gradually transforms it into the syntax rules , starting from the low level rules until high level rules are met.

  ## Generating parsers automatically.
  Creating a parser by hand can be quite a task as it requires a deep understanding of parsing. 
There are tools that can generate a parser. You feed them the grammar of your language, its vocubulary and syntax rules and then they generate a wroking parser.
Webkit uses two parser generators: _Flex_ for creating a lexer and _Bison_ for creating a parser.
Flex input is a file containing regular expression definitions of the tokens. Bison's input is the language syntax rules in BNF format.

 ## HTML parser.
  The HTML parser parses the HTML markup into a parse tree.
  ##The HTML grammar definition.
  The vocubulary and syntax of an HTML document are defined in specifications created by the W3C organization.
  ## HTML is not a context free grammar.
  Grammar syntax can be defined formally using formats like BNF.

Unfortunately all the conventional parser topics do not apply to HTML . HTML cannot easily be defined by a context free grammar that parsers need.

  There is a formal format for defining HTML–DTD (Document Type Definition)–but it is not a context free grammar.
The difference is that the HTML approach is more "forgiving": it lets you omit certain tags (which are then added implicitly), or sometimes omit start or end tags, and so on. On the whole it's a "soft" syntax, as opposed to XML's stiff and demanding syntax.

  This seemingly small detail makes a world of a difference. On one hand this is the main reason why HTML is so popular: it forgives your mistakes and makes life easy for the web author. On the other hand, it makes it difficult to write a formal grammar. In summary, HTML cannot be parsed easily by conventional parsers, since its grammar is not context free. HTML cannot be parsed by XML parsers. 

  ## HTML DTD
   HTML definition is in a DTD format. This format is used to define languages of the SGML family. The format contains definitions for all allowed elements, their attributes and hierarchy. As we saw earlier, the HTML DTD doesn't form a context free grammar.

There are a few variations of the DTD. The strict mode conforms solely to the specifications but other modes contain support for markup used by browsers in the past. The purpose is backwards compatibility with older content.

 ## DOM
  The output tree (the "parse tree") is a tree of DOM element and attribute nodes. DOM is short for **_Document Object Model_**. It is the object presentation of the HTML document and the interface of HTML elements to the outside world like JavaScript.
The root of the tree is the "Document" object. The tree is constructed of elements that implement one of the DOM interfaces. Browsers use concrete implementations that have other attributes used by the browser internally.

  ## The parsing algorithm.
  HTML cannot be parsed using the regular top down or bottom up parsers mainly because:
    1.The forgiving nature of the language.
    2. The fact that browsers have traditional error tolerance to support well known cases of invalid HTML.
   3. The parsing process is reentrant. For other languages, the source doesn't change during parsing, but in HTML, dynamic code (such as script elements containing document.write() calls) can add extra tokens, so the parsing process actually modifies the input.

Unable to use the regular parsing techniques, browsers create custom parsers for parsing HTML. 

The algorithm consists of two stages: tokenization and tree construction.

Tokenization is the lexical analysis, parsing the input into tokens. Among HTML tokens are start tags, end tags, attribute names and attribute values.

The tokenizer recognizes the token, gives it to the tree constructor, and consumes the next character for recognizing the next token, and so on until the end of the input. 

   ## The tokenization algorithm.
  The algorithm's output is an HTML token. The algorithm is expressed as a state machine. Each state consumes one or more characters of the input stream and updates the next state according to those characters. The decision is influenced by the current tokenization state and by the tree construction state. This means the same consumed character will yield different results for the correct next state, depending on the current state.
 For example: ''\<html>, The initial state is the **Data state**. When the (<) character is encountered, the state is changed to **Tag open state** . Consuming an a-z character causes creation of a **Start tag token**, the state is changed to "Tag name state". We stay in this state until the (>) character is consumed. Each character is appended to the new token name. In our case the created token is an html token. When the > tag is reached, the current token is emitted and the state changes back to the **Data state**.

  ## Tree construction algorithm.
   When the parser is created the Document object is created. During the tree construction stage the DOM tree with the Document in its root will be modified and elements will be added to it. Each node emitted by the tokenizer will be processed by the tree constructor. For each token the specification defines which DOM element is relevant to it and will be created for this token. The element is added to the DOM tree, and also the stack of open elements. This stack is used to correct nesting mismatches and unclosed tags. The algorithm is also described as a state machine. The states are called "insertion modes".

  ## Actions when the parsing is finished.

At this stage the browser will mark the document as interactive and start parsing scripts that are in "deferred" mode: those that should be executed after the document is parsed. The document state will be then set to "complete" and a "load" event will be fired. 

  ## Browser's error tolerance.
  Browsers fix any invalid content and go on and that's whyn ypu may never find an "Invalid syntax" error on an HTML page.
  Error handling is quite consistent in browsers, even though it hasn't been part of HTML specifications. Like bookmarking and back/forward buttons it's just something that developed in browsers over the years. There are known invalid HTML constructs repeated on many sites, and the browsers try to fix them in a way conformant with other browsers.

  # CSS parsing
 Well, unlike HTML, CSS is a context free grammar and can be parsed using the types of parsers described in the introduction. In fact the CSS specification defines CSS lexical and syntax grammar. 

  ## The Order of parsing scripts.

   ## Scripts
 The model of the web is synchronous. Authors expect scripts to be parsed and executed immediately when the parser reaches a \<script> tag. The parsing of the document halts until the script has been executed. If the script is external then the resource must first be fetched from the network–this is also done synchronously, and parsing halts until the resource is fetched. This was the model for many years and is also specified in HTML4 and 5 specifications. Authors can add the "defer" attribute to a script, in which case it will not halt document parsing and will execute after the document is parsed. HTML5 adds an option to mark the script as asynchronous so it will be parsed and executed by a different thread.

  ## Speculative parsing

Both WebKit and Firefox do this optimization. While executing scripts, another thread parses the rest of the document and finds out what other resources need to be loaded from the network and loads them. In this way, resources can be loaded on parallel connections and overall speed is improved. Note: the speculative parser only parses references to external resources like external scripts, style sheets and images: it doesn't modify the DOM tree–that is left to the main parser.

  ## Style sheets

Style sheets on the other hand have a different model. Conceptually it seems that since style sheets don't change the DOM tree, there is no reason to wait for them and stop the document parsing. There is an issue, though, of scripts asking for style information during the document parsing stage. If the style is not loaded and parsed yet, the script will get wrong answers and apparently this caused lots of problems. It seems to be an edge case but is quite common. Firefox blocks all scripts when there is a style sheet that is still being loaded and parsed. WebKit blocks scripts only when they try to access certain style properties that may be affected by unloaded style sheets.
 
  ## Render tree construction

  While the DOM tree is being constructed, the browser constructs another tree, the render tree. This tree is of visual elements in the order in which they will be displayed. It is the visual representation of the document. The purpose of this tree is to enable painting the contents in their correct order.

  Firefox calls the elements in the render tree "frames". WebKit uses the term renderer or render object.
A renderer knows how to lay out and paint itself and its children.


  # Layout.
  When the renderer is created and added to the tree, it does not have a position and size. Calculating these values is called layout or reflow.

HTML uses a flow based layout model, meaning that most of the time it is possible to compute the geometry in a single pass. Elements later ``in the flow'' typically do not affect the geometry of elements that are earlier ``in the flow'', so layout can proceed left-to-right, top-to-bottom through the document. There are exceptions: for example, HTML tables may require more than one pass (3.5).

  The coordinate system is relative to the root frame. Top and left coordinates are used.

  Layout is a recursive process. It begins at the root renderer, which corresponds to the \<html> element of the HTML document. Layout continues recursively through some or all of the frame hierarchy, computing geometric information for each renderer that requires it.

  The position of the root renderer is 0,0 and its dimensions are the viewport–the visible part of the browser window.
All renderers have a "layout" or "reflow" method, each renderer invokes the layout method of its children that need layout.

  ## Dirty bit system

  In order not to do a full layout for every small change, browsers use a "dirty bit" system. A renderer that is changed or added marks itself and its children as "dirty": needing layout.

  There are two flags: "dirty", and "children are dirty" which means that although the renderer itself may be OK, it has at least one child that needs a layout.

  ##Global and incremental layout

  Layout can be triggered on the entire render tree–this is "global" layout. This can happen as a result of:

   1.A global style change that affects all renderers, like a font size change.
   2.As a result of a screen being resized
   Layout can be incremental, only the dirty renderers will be laid out (this can cause some damage which will require extra layouts). 
Incremental layout is triggered (asynchronously) when renderers are dirty. For example when new renderers are appended to the render tree after extra content came from the network and was added to the DOM tree.

  ## Asynchronous and Synchronous layout

 Incremental layout is done asynchronously. Firefox queues "reflow commands" for incremental layouts and a scheduler triggers batch execution of these commands. WebKit also has a timer that executes an incremental layout–the tree is traversed and "dirty" renderers are layout out. 
Scripts asking for style information, like "offsetHeight" can trigger incremental layout synchronously. 
Global layout will usually be triggered synchronously. 
Sometimes layout is triggered as a callback after an initial layout because some attributes, like the scrolling position changed.
Optimizations

  When a layout is triggered by a "resize" or a change in the renderer position(and not size), the renders sizes are taken from a cache and not recalculated.. 
In some cases only a sub tree is modified and layout does not start from the root. This can happen in cases where the change is local and does not affect its surroundings–like text inserted into text fields (otherwise every keystroke would trigger a layout starting from the root).
The layout process

The layout usually has the following pattern:

  1.Parent renderer determines its own width.
  2.Parent goes over children and:
    1.Place the child renderer (sets its x and y).
    2.Calls child layout if needed–they are dirty or we are in a global layout, or for some other reason–which calculates the child's height.
  3.Parent uses children's accumulative heights and the heights of margins and padding to set its own height–this will be used by the parent renderer's parent.
  4.Sets its dirty bit to false.
Firefox uses a "state" object(nsHTMLReflowState) as a parameter to layout (termed "reflow"). Among others the state includes the parents width. 
The output of the Firefox layout is a "metrics" object(nsHTMLReflowMetrics). It will contain the renderer computed height.

  ## Line Breaking

When a renderer in the middle of a layout decides that it needs to break, the renderer stops and propagates to the layout's parent that it needs to be broken. The parent creates the extra renderers and calls layout on them.

  ## Painting

In the painting stage, the render tree is traversed and the renderer's "paint()" method is called to display content on the screen. Painting uses the UI infrastructure component.

  ## Global and Incremental

Like layout, painting can also be global–the entire tree is painted–or incremental. In incremental painting, some of the renderers change in a way that does not affect the entire tree. The changed renderer invalidates its rectangle on the screen. This causes the OS to see it as a "dirty region" and generate a "paint" event. The OS does it cleverly and coalesces several regions into one. In Chrome it is more complicated because the renderer is in a different process then the main process. Chrome simulates the OS behavior to some extent. The presentation listens to these events and delegates the message to the render root. The tree is traversed until the relevant renderer is reached. It will repaint itself (and usually its children).

  ## The painting order

CSS2 defines the order of the painting process. This is actually the order in which the elements are stacked in the stacking contexts. This order affects painting since the stacks are painted from back to front. The stacking order of a block renderer is:
  1.background color
  2.background image
  3.border
  4.children
  5.outline

  ## Firefox display list

  Firefox goes over the render tree and builds a display list for the painted rectangular. It contains the renderers relevant for the rectangular, in the right painting order (backgrounds of the renderers, then borders etc). That way the tree needs to be traversed only once for a repaint instead of several times–painting all backgrounds, then all images, then all borders etc.
Firefox optimizes the process by not adding elements that will be hidden, like elements completely beneath other opaque elements.

  ## WebKit rectangle storage

  Before repainting, WebKit saves the old rectangle as a bitmap. It then paints only the delta between the new and old rectangles. 
  ##Dynamic changes

  The browsers try to do the minimal possible actions in response to a change. So changes to an elements color will cause only repaint of the element. Changes to the element position will cause layout and repaint of the element, its children and possibly siblings. Adding a DOM node will cause layout and repaint of the node. Major changes, like increasing font size of the "html" element, will cause invalidation of caches, relayout and repaint of the entire tree.
   