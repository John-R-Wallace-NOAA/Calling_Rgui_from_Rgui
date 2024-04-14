
### Calling Rgui from Rgui can be done in the following way:
   
     # First note that the '>' operator in the Windows command shell, or Unix-a-likes shells, creates a file with echo(ed) line 
     #    and the '>>' appends what is echo(ed) onto the next line.
     # For now this has been tested in Windows R, but a similar thing can be done in Unix-a-likes using system() instead of shell().
     
     # -- Create the local initial file '.Rprofile' which Rgui.exe will run when started --
     # The listed packages below do not immediately load, but are standard in a normal Rgui startup.
     #   They are loaded by the time a prompt is given, but that hasn't happen here, so these packages are loaded explictly.
     shell("echo library(stats); library(graphics); library(grDevices); library(utils); library(datasets) > .Rprofile")
     shell("echo simplePlot = function(minX, maxX, col) { plot(minX:maxX, col = col) } >> .Rprofile")  # The classic R assignment operator "<-" does not work on the echo line.
     shell("echo simplePlot(1, 20, col = 'green') >> .Rprofile")
     shell("echo Sys.sleep(7) >> .Rprofile")
     shell("echo quit('no',,FALSE) >> .Rprofile")
     
     
     # -- Create run.bat and start it with the 'wait' argument. --
     #        Remove the 'wait' argument (and comment out or remove < shell("del run.bat") >) for parallel  processing.
     # shell("echo cd C:\\A_folder_where_a_.Rprofile_is_already_setup > run.bat") # Using this line allows running the next 4 lines of R code from any working directory.
     shell("echo C:\\R\\R\\bin\\x64\\Rgui.exe --no-save --no-restore --no-site-file --no-environ > run.bat")
     shell("echo exit >> run.bat")
     shell("start /wait run.bat")
     shell("del run.bat")
     
     
     
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
   
   
   
   # Anything can be now done inside of .Rprofile:
    
   # First save the simpleplot function to a file
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
   
