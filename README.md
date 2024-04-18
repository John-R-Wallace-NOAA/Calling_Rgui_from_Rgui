
### Calling Rgui from Rgui, for batch R graphic capabilities and stopping memory leaks, can be done in the following way:
     
     # First note that the '>' operator in the Windows command shell, or Unix-a-likes shells, creates a file with the echo(ed)
     #    line and '>>' appends what is echo(ed) to the next empty line.
     
     # -- Create the local initial file '.Rprofile' which Rgui.exe will run when started --
     # The listed packages below do not immediately load, but are standard in a normal Rgui startup.
     #   They are loaded by the time a prompt is given, but that hasn't happen here, so these packages are loaded explictly.
     shell("echo library(stats); library(graphics); library(grDevices); library(utils); library(datasets) > .Rprofile")
     # The classic R assignment operator "<-" does not work on the echo line.
     shell("echo simplePlot = function(minX, maxX, col) { plot(minX:maxX, col = col) } >> .Rprofile")  
     shell("echo simplePlot(1, 20, col = 'green') >> .Rprofile")
     shell("echo Sys.sleep(7) >> .Rprofile")
     shell("echo quit('no',,FALSE) >> .Rprofile")
     
     
     # -- Create run.bat, changing the Rgui path as needed, and start it with the 'wait' argument. --
     #    Remove the 'wait' argument (and comment out or remove the < shell("del run.bat") > line) for parallel processing.
     # Using the change directory (cd) line below allows running the next 4 lines of R code from any working directory,
     #    but the .Rprofile would already need to be setup in the destination directory. 
     #    If used, the Rgui line would then need a ">>" not the ">".
     # shell("echo cd C:\\A_folder_where_a_.Rprofile_is_already_setup > run.bat") 
     shell("echo C:\\R\\R\\bin\\x64\\Rgui.exe --no-save --no-restore --no-site-file --no-environ > run.bat")
     shell("echo exit >> run.bat")
     shell("start /wait run.bat")
     shell("del run.bat") # Comment out this line for debugging
      
     
     
     # Try a sequential loop
     for(i in 1:2) {
     
       shell("echo library(stats); library(graphics); library(grDevices); library(utils); library(datasets) > .Rprofile")
       shell("echo simplePlot = function(minX, maxX, col) { plot(minX:maxX, col = col) } >> .Rprofile")
       shell(paste0("echo simplePlot(1, c(10, 20)[", i, "], col = c('green', 'dodgerblue')[", i, "]) >> .Rprofile"))
       shell("echo Sys.sleep(12) >> .Rprofile")
       shell("echo quit('no',,FALSE) >> .Rprofile")
       
       shell("echo C:\\R\\R\\bin\\x64\\Rgui.exe --no-save --no-restore --no-site-file --no-environ > run.bat")
       shell("echo exit >> run.bat")
       shell("start /wait run.bat")
       shell("del run.bat")
     }
     
     
     # Try parallel processing
     for(i in 1:2) {
     
       shell("echo library(stats); library(graphics); library(grDevices); library(utils); library(datasets) > .Rprofile")
       shell("echo simplePlot = function(minX, maxX, col) { plot(minX:maxX, col = col) } >> .Rprofile")
       shell(paste0("echo simplePlot(1, c(10, 20)[", i, "], col = c('green', 'dodgerblue')[", i, "]) >> .Rprofile"))
       shell("echo Sys.sleep(12) >> .Rprofile")
       shell("echo quit('no',,FALSE) >> .Rprofile")
       
       shell("echo C:\\R\\R\\bin\\x64\\Rgui.exe --no-save --no-restore --no-site-file --no-environ > run.bat")
       shell("echo exit >> run.bat")
       shell("start run.bat")
     }
   
   
    # Normal R operations can done inside of .Rprofile, e.g. source()ing and load()ing:
     
    # First save the simpleplot function to an R file
    shell("echo simplePlot = function(minX, maxX, col) { plot(minX:maxX, col = col) } > simplePlot.R")
    
     
    shell("echo library(stats); library(graphics); library(grDevices); library(utils); library(datasets) > .Rprofile")
    shell("echo source('simplePlot.R') >> .Rprofile")
    
    shell("echo save(simplePlot, file = 'simplePlot.RData') >> .Rprofile")
    shell("echo load('simplePlot.RData') >> .Rprofile")
    shell("echo simplePlot(1, 20, col = 'green') >> .Rprofile")
   
    shell("echo save.image() >> .Rprofile")
    
    shell("echo Sys.sleep(7) >> .Rprofile")
    shell("echo quit('no',,FALSE) >> .Rprofile") # Quit without user interaction
   
    shell("echo C:\\R\\R\\bin\\x64\\Rgui.exe --no-save --no-restore --no-site-file --no-environ > run.bat")
    shell("echo exit >> run.bat")
    shell("start run.bat")
   
See this [script]([https://github.com/John-R-Wallace-NOAA/FishNIRS/tree/main/R_Scripts/iPLS_Batch_Loop](https://github.com/John-R-Wallace-NOAA/FishNIRS/blob/main/R_Scripts/iPLS%2C%20NN%20Model%20Batch%20Self%20Call%20Loop.R)) for an example of stopping memory leaks by calling Rgui from Rgui, where R's 
memory garbage collection, gc(), and TensorFlow's, k_clear_session(), failed to do so.
