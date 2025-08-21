# langgraph app with fastapi awsgi server
langgraph app serve using fastapi server with uvicorn(awsgi)

about project : React agent with tavily search tool , so agent get information from tavily search tool call.

how it works 

 [1]. client(Reactjs app) and server    
 
>client use EventSource() method to request server with input and checkpoint_id (here checkpoint_id use for persist state in  langgraph app across multiple invoke )
>server get request with  message and checkpoint_id
>server StreamingResponse contain generate_chat_stream function.
>generate_caht_stream funtion invoke graph with message and check[point_id and yield graph stream chunk
>(yield format is    "data:{"type":"chunk"/"content"/"end","content":"AIMessage" or "ToolMessage"}"
>client get reponse back from server.

[2]. Render message and ui based on  Message type and id 
>setMessage for stream chunk and setState for stream state from server
>client render message and ui based on message type(based on message type we render ui) and id(for streaming chunk and partial response from agent)
>(important) client  render streaming chunk to bubble and after get state render and setMessage("") flush , and setState() render to ui


[3]. graph stream method  : .stream() , .astream() , astream_events()
>output = graph.stream(input,config,stream_mode=["messages","values","custom"])/.astream(input,config,stream_mode)/.astream_events(input,config,stream_mode)   
>diffrent graph run method give diffrent output
>stream_mode =['custom','values','messages','update' ,'debug']

[4] graph.stream()/astream()/astream_events() run method support stream_mode:
>'messages', 'custom' mode is use for streaming ouput chunk from graph  (chunk from inside graph node)
>'values','update' mode streaming state at itermeadiate graph step (when node return state and state is update) step  

[5]graph.invoke()     // final state reponse or when  graph interrupt 
[Note: interrupt message can be output when graph is interrupt ]
>graph output state end of graph or when interrupt


[6].graph workflow
graph contain simple React agent with tavily search Tool calling.


[7] how generative ui work
>get_stream_writer() method play a important role in Generative ui, custom message stream (partial_response )
>get_stream_writer() use for stream custom  chunk , after stream complate aggregated custom_chunk return from node ,so state contain stream chunk messages.
>get_stream_writer() work with stream_mode='custom'
>langchain LCEL chain or langchain llm can streaming chunk when stream_mode='messages
