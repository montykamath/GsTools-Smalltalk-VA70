Paul Berglund did a port of GsTools to VA 7.0.
There is still some more work to do but this is a nice start. 

PGB 12-6-05

Keep in mind that this is by no means a completed port; there's a lot of stuff
that I haven't even tried to test.  It does pass the "will it load" test, however,
and the singleton browser that I can't live without seems to be working. 

Summary of changes:

1. I removed #compile:in:notifying: from EtMethodsBrowser and EtClassBrowser.  
They are now implemented by StsPowerTools, so the call to #gsLogMethodCreation: 
will have to be added in some other way, but I have not done so yet.

2. StsPowerTools implements #menuEdit on EtBrowser, EtInspector and EtWorkspace, 
so I put the code to add the GsTools menu picks in #stsEditMenu: instead.

3. EtCodeWindow>>#initializeAfterRealize was breaking the StsPowerTools code 
coloring mechanism somehow, so for now I commented out the line that was causing 
the trouble.

4. There was a walkback in EtWindow>>#gstShowSystemWindowsList plus it was 
listing more windows than made sense to me, so I made a quick hack to fix that. 

5. I added #selectedMethods and #selectedClasses to GstWorkspaceOrganizer, as I 
was getting walkbacks without them.

6. For the time being I commented out GsTools>>#updateBrowserMappings.  The browsers 
that existed when GsTools were written have now been enhanced so I expect that either 
the Gs* enhancements aren't needed anymore or else they should be moved to subclass 
(and extend) the Sts* versions rather than the older unenhanced ones they subclass now.

7. GstWorkspaceOrganizer>>#openOrganizer was looking for the organizer by seeing 
which of CwShell allShells had a title of 'Workspace Organizer'.  But for some reason 
there is now some object in CwShell allShells that doesNotUnderstand #title so as a quick 
and dirty fix I added a "respondsTo:" check.

8. I removed Character>>#space and Collection>>#do:separatedBy: from GsBaseExtensions
because they now appear to be implemented in the VA base.

PGB 2-1-06

9. Fixed a walkback which would occur if you tried to send methods from a changes
browser to a singleton methods browser. 

PGB 11-7-06

10. Fixed a double "do you want to save?" prompt when closing a methods browser.
