BPQuery : Natural language interface to query knowledge bases representing business processes using logic programming
Author : Stefan de Lavergne (Stefan_524450)

---------------------------------------------------------------------------------------------------------------------------------------

The java resources are divided into two packages :
- xmlParser : contains the .java files that are parsing the diagram to build the knowledge base in prolog
- nlp : contains the .java files that process the natural language query, build the prolog query out of it and query the knowledge base

---------------------------------------------------------------------------------------------------------------------------------------

To start the application, run Main.jsp : http://localhost:8080/BPTestUI/Main.jsp


First, select a .bpmn or .xml file from your computer. FileChooserServlet.java will verify the extension of the file. If it is recognized, it will run ParserServlet.java. ParserServlet.java will then run Parser.java to parse the xml file and build the prolog knowledge base. The prolog file will be available in the same directory. If the process contains XOR-split gateways, the application will ask the user to specify the conditions. The conditions are then added to the prolog file and the user can type his query. To help with the query, the user can display the agents and activities found in the process by clicking on the corresponding button.

The query must follow the pattern <agent><action><activity>? Here are some examples :
- Who does what ? ---------------> <who><do><what>
- Who is delivering the pizza? --> <who><do><deliverthepizza>
- Who receives what? ------------> <who><receive><what>
- What does the call center do? -> <callcenter><do><what>

You can also type another kind of query : <what><follow><activity>? Example :
- What follows answering customer call?

MainServlet.java will run the query. First, it will ask Chunker.java to analyze the sentence using the Stanford Typed Dependencies and build the Pattern, an object containing an agent, an action and an activity. Then, the pattern is sent to the QueryEngine.java. This class will match the pattern with a prolog query pattern. In the process, it will match the agent or the activity contained in the pattern with the known agents and activities from the prolog knowledge base, using the Matcher.java file. A string answer is built that will contain the result corresponding to the query.

Once the prolog query is formed, QueryEngine.java will query the prolog knowledge base using JPL interface. The results are analyzed and stored into an object Result. If there are any results, MainServlet.java will display them on the interface.