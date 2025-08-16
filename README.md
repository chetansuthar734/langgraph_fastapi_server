# langgraph_fastapi_server
langgraph app  serve using fastapi server




for start application run locally
pull repo 
cd ./langgraph_fastAPI_server
1.pip install -e .
2.create .env file contains google_api_key and tavily_api_key
3.uvicorn app:app       //run  awsgi server

 

how it works 

 [1]. client(Reactjs app) and server    
 
>client use EventSource() method to request server with input and checkpoint_id (here checkpoint_id use for persist state in  langgraph app across multiple invoke )
>server get request with input and checkpoint_id ( based on checkpoint_id graph invoke and yield to StreamingResponse() )
>server response using StreamingRespose() sse event (response format is    "data:{"type":"content" or "end,"content":"AIMessage" or "ToolMessage"}"
>client get reponse back from server.
>client render message and ui based on message type(based on message type we render ui) and id(for streaming chunk and partial response from agent)

[2]. graph run type   output = graph.stream(input , config, stream_mode )/.astream()/.astream_events()   
>diffrent graph run method give diffrent output
>stream_mode =['custom','values','messages','update' ,'debug']


[3] graph.stream()/astream()/astream_events() run method support stream_mode:
>'messages', 'custom' mode is use for streaming ouput chunk from graph  (chunk from inside graph node)
>'values','update' mode streaming state at itermeadiate graph step (when node return state )  step  

[4]graph.invoke()
>graph output state end of graph


