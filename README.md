<h1>EC-Admin</h1>

<p>EC-Admin is a collection of administrative procedures to help you
manage your server. Its inception was sparked by requests from customers
during my Professional Services engagements. It has grown over the years to
what it is now.</p>

<p>It contains the following modules:</p>
<ul>
<li><a href="#health">System Health</a></li>
<ul>
<li>agentMemoryConfiguration</li>
<li>testResources</li>
<li>performanceMetrics</li>
</ul>

<li><a href="#look">Look and Feel</a></li>
<ul>
<li>changeBannerColor</li>
</ul>

<li><a href="#jobs">Jobs and Workspaces management</a></li>
<ul>
<li>jobsCleanup</li>
<li>jobsCleanup_byResult</li>
<li>deleteWorkspaceOrphans</li>
<li>deleteObjects</li>
</ul>

<li><a href="#plugin">Plugins</a></li>
<ul>
<li>synchronizePlugins</li>
<li>createPluginFromProject (deprecated)</li>
<li>projectAsCode</li>
</ul>

<li><a href="#artifacts">Artifacts management</a></li>
<ul>
<li>artifactRepositorySynchronization</li>
<li>artifactsCleanup</li>
<li>artifactsCleanup_byQuantity</li>
</ul>

<li><a href="#backup">Object export backup and restore</a></li>
<ul>
<li>saveProjects</li>
<li>saveAllObjects</li>
<li>restoreObjects</li>
</ul>

<li><a href="#schedules">Schedules</a></li>
<ul>
<li>schedulesDisable</li>
<li>schedulesEnable</li>
</ul>

<li><a href="#semaphore">Semaphore management</a></li>
<ul>
<li>acquireSemaphore</li>
<li>releaseSemaphore</li>
</ul>

<li><a href="#license">License Logger</a></li>
<li><a href="#postp">postp debugger helper</a></li>
<li><a href="#comm">Communication</a></li>
<li><a href="#misc">miscellaneous</a></li>
<ul>
<li>findJobId</li>
</ul>

</ul>

<h2><a name="installation"></a>Installation</h2>

<p>This collection of procedures can be found on our
<a href="https://github.com/electriccommunity/EC-Admin">GitHub
repository</a>. It is available as a plugin delivered
in the form of a .jar file or you can simply get the source code
and recompile it yourself.</p>

<p>To install the plugin, use one the following methods:</p>
<ul>
<li>the "Install from File/URL" tab in the administration/Plugins</li>
<li>the Perl "$ec->installPlugin()" API.</li>
<li> the CLI client "ectool installPlugin --force 1 EC-Admin.jar</li>
</ul>
<p>Don't forget to promote the plugin after installation.</p>

<p>To install on 4.0.x or or 4.1.x, import the EC_4.0.xml file. Be
aware that some features are not present as the original project
makes use of createJobStep() API which was introduced in 4.2.0:<br/>
ectool import /path_to/EC-Admin_for_EC_4.0.xml --disableSchedules 1</p>

<p>IMPORTANT: For all parameter descriptions below, required
parameters are shown in <span class="required">bold italics</span>.</p>

<h1><a name="health"></a>System Health</h1>
<h2>agentMemoryConfiguration</h2>
<p>This procedure allows you to change the Java heap memory setting of your
agents. Electric Cloud support recommend increasing the default for agents
with a heavier load like the repository server agent, gateway agent and
web server agent.</p>
<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>agent</td>
<td>The name of the resource on which the agent is located.</td>
</tr>
<tr>
<td class='required'>init memory</td>
<td>The amount of init memory to assign to the Java heap. It can be
indicated in Mb or as a % of the toal memory.</td>
</tr>
<tr>
<td class='required'>max memory</td>
<td>The maximum amount of memory to assign to the Java heap. It can be
indicated in Mb or as a % of the toal memory.</td>
</tr>
<tr>
<td>restartAgent</td>
<td>On Linux the agent service is restarted, on Windows the box is
rebooted.</td>
</tr>
</tbody>
</table>

<h2>testResources</h2>
<p>This procedure is simply to test connection to all your resources or a
specific pool. This will create simple step to run an echo command on
alive resources.</p>
<table class="grid">
<thead>
<tr><th>Parameter</th><th>Description</th></tr>
</thead>
<tbody>
<tr>
<td>pool</td>
<td>Name of a pool. If empty, all resources will be tested</td>
</tr>
</tbody>
</table>

<h2><a name="perf"></a>performanceMetrics</h2>
<p>This procedure returns some information about your
ElectricCommander server like number of processors, total RAM,
available RAM,
ping times with agents, relative performance of agents, ...
In addition it checks the amount of time spent running steps on the server
local agents as this should be minimized as much as possible.</p>

<h1><a name="look"></a>Look and Feel</h1>
<h2>changeBannerColor</h2>
<p>If you work on multiple Commander servers like a development and a
production instances, this procedure is for you. It allows you to change the color of the top banner and the logo to make it easier to identify your different servers. Feel free
to <a href="mailto:lrochette@electric-cloud.com">send me</a> new ones if you have a more artistic touch than me. Or better
push your changes to
<a href="https://github.com/electriccommunity/EC-Admin">GitHub</a>.</p>

<table class="grid">
<thead>
<tr><th>Parameter</th><th>Description</th></tr>
</thead>
<tbody>
<tr>
<td class='required'>color</td>
<td>Choose a color for your top banner</td>
</tr>
<tr>
<td>logo</td>
<td>Choose a set of pre-rendered logo to indicate
if you server is of type DEV, SDK, TEST or PROD, in black or white to
go along different background colors.</td>
</tr>
<tr>
<td>logoFile</td>
<td>The path on the Commander server to upload your own logo. If this value is not blank it will overwritten the logo parameter above.</td>
</tr>
<tr>
<td>webResource</td>
<td>The name of the resource running on your webserver to allow the copy of the logo and banner color files.</td>
</tr>
</tbody>
</table>
<p><b>Note:</b> if you run your server and agent as different user (as recommended), the agent won't be able to modify the following files:</p>
<ul>
<li>INSTALL_DIR/apache/htdocs/commander/images/logo.gif</li>
<li>INSTALL_DIR/apache/htdocs/commander/lib/images/frame_bannerBg.gif (version 4.x and later)</li>
<li>INSTALL_DIR/apache/htdocs/commander/lib/styles/StdFrame.css (version 5.4 )</li>
<li>INSTALL_DIR/apache/htdocs/commander/styles/StdFrame.css (version 6.0 and later)</li>
</ul>
<p>Those file permissions need to be opened so the agent user can modify them.</p>

<img src="src/htdocs/images/help/changeBannerColor.png"
alt="changeBannerColor parameters" />
<p>will produce</p>
<img src="src/htdocs/images/help/RedDev.png"
alt="Red DEV example" />

<h1><a name="jobs"></a>Jobs and Workspaces management</h1>
<p>This set of procedures will help you manage your old jobs and the
associated workspaces.</p>

<h2>jobsCleanup:</h2>
<p>A procedure to delete jobs older than a specified number of
days. It will also delete associated workspace job directories.</p>

<table class="grid">
<thead>
<tr><th>Parameter</th><th>Description</th></tr>
</thead>
<tbody>
<tr>
<td class='required'>olderThan</td>
<td>Indicate the number of days after which jobs are deleted</td>
</tr>
<tr>
<td class='required'>jobType</td>
<td>Define what jobs should be deleted: Error, Warning or all.</td>
</tr>
<tr>
<td>jobProperty</td>
<td>Name of a property attached to your jobs. If this property is set, your job won't be deleted no matter how old it is.</td>
</tr>
<tr>
<td>jobPatternMatching</td>
<td>A SQL pattern matching to automatically preserve some jobs. The pattern must solve for a LIKE statement.</td>
</tr>
<tr>
<td class='required'>delete</td>
<td>If this option is not set, only report is done. No real data are deleted</td>
</tr>
<tr>
<td class='required'>computeUsage</td>
<td>Check off to skip the computation of the number of steps and the file space used for the job. This will speed up the execution.</td>
</tr>
</tbody>
</table>

<img src="src/htdocs/images/help/jobsCleanup.png"
alt="jobsCleanup parameters" />

<h2>jobCleanup_byResult:</h2>
<p>A procedure to delete jobs older than a specified number of days.
It will also delete associated workspace job directories. It will keep the
number of successful, failed and warning jobs you entered. You can also
provide a property name, if this property is attached to the job, it won't be
deleted no matter how old. A report only mode prevents any real deletion.</p>

<h2>deleteWorksapceOrphans:</h2>

<p>A procedure to crawl a workspace directory to find orphan jobs
(directories without a matching job) on a specified resource.</p>

<h2>deleteObjects:</h2>
<p>A procedure to quickly delete jobs or workflows older than a
specified number of days. It will <b>not</b> delete associated workspace job
directories. This is for customers with huge job or workflow database.</p>


<h1><a name="plugin"></a>Plugins</h1>
<h2>Plugin Synchronization</h2>
<p>This procedure synchronizes plugins between the server and
a resource. Plugins are uploaded first as artifacts if it has not been
done already and then downloaded on the resource if it does not
already exist in the plugins directory.</p>

<h2>Plugin creation</h2>
<h3>createPluginFromProject</h3>
<p><b>Note:</b> This procedure requires access to the jar or zip
executable in the PATH.</p>
<p>The procedure "createPluginFromProject" allows the transformation of a
project into a plugin. The parameters are the following:</p>
<table class="grid">
<thead>
<tr>
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>

<tbody>
<tr>
<td class='required'>Project</td>
<td>The name of the project to transform into a plugin</td>
</tr>
<tr>
<td>Plugin</td>
<td>The name of the plugin. If left empty, it will default to
the project name.</td>
</tr>
<tr>
<td>Category</td>
<td>The category of the plugin. It helps filter the plugin list in
the Plugins page and in the picker list.</td>
</tr>
<tr>
<td  class='required'>Version</td>
<td>Version of the plugin to create. A build number is automatically
added (after incrementation) to ensure uniq version numbers.</td>
</tr>
<tr>
<td>Description</td>
<td>The description of the plugin to display in the main
plugin page. If left empty it will default to the
description of the project.</td>
</tr>
<tr>
<td>Author</td>
<td>Name of he author of the project to transform into a plugin.</td>
</tr>
<tr>
<td>Email</td>
<td>The email of the author.</td>
</tr>
<tr>
<td>Comment</td>
<td>if this parameter is not empty, it will be added to the CHANGELOG for traceability.</td>
</tr>
<tr>
<td>fixSelfReferences</td>
<td>Replace self references to the project in sub-prceodure calls by an empty string to be sure the plugins= created will not call the original project instead of itself.</td>
</tr>
<tr>
<td>createArtifact</td>
<td>Save the new created plugin as an artifact version.</td>
</tr>
<tr>
<td>installPlugin</td>
<td>A Boolean to install or not the plugin after creation</td>
</tr>
<tr>
<td>promotePlugin</td>
<td>A Boolean to promote or not the plugin after installation</td>
</tr>
<tr>
<td>moveJobs</td>
<td>A Boolean to move or not jobs from the previous
version into the current one upon promotion.</td>
</tr>
</tbody>
</table>

<img src="src/htdocs/images/help/createPluginFromProject.png"
alt="createPluginFromProject parameters" />

<p>At the project level, you can define the following properties to
alter the behavior of the plugin creation:</p>
<dl>
<dt><b>ec_visibility:</b></dt>
<dd>Choose <b>all</b> to have the plugin shows in the project
list (like EC-Admin), or <b>pickListOnly</b> (default) to
have your procedures available only through the plugin
procedure picker (when you choose plugin as a step).</dd>
<dt><b>help:</b></dt>
<dd>This property allows you to create a Help page associated
with your plugin. The page you are currently reading is
created this way so feel free to explore the help property
in the EC-Admin plugin/project for an example.</dd>
<dt><b>promoteAction</b></dt>
<dd>This property would contain some Perl code to take action
when the plugin change state. Look at the ec_setup property
in the plugins installed on your system to get a better idea
of what you can accomplish.<br/>
For example to set a property hwne the plugin is pomoted
you could add the the following code to the promoteAction
procedure:<br/>
<pre><code>
if ( $promoteAction eq 'promote' ) {
$batch->setProperty( "/projects/$pluginName/addedByPromote", "123" );
}
</code></pre>
</dd>
</dl>
<p>In addition each procedure can modify the behavior with the
following 2 properties:</p>
<dl>
<dt><b>exposeToPlugin:</b></dt>
<dd>This property if set to 1 will add the procedure to
ec_setup to be shown in the procedure picker list when
choosing a plugin step.</dd>
<dt><b>descriptionForPlugin:</b></dt>
<dd>This property will be shown as the description in the
picker list mentioned above. If it's empty or non-existent,
the description will be description of the procedure
itself.</dd>
</dl>

<h3>projectAsCode</h3>

<p><b>Note:</b> This procedure requires access to a resource with the
Commander SDK installed.</p>

<p>This procedure is an extension of createPluginFromProject. It also
creates a plugin but instead of simply exporting the project, it
"explodes" each step in its own file for finer granularity check in
in your favorite SCM tool.<br/>You can also add automatically a
mechanism to add credential/configuration management to you plugin.<br/>
All the properties associated with createPluginFromProject still
apply.</p>

<table class="grid">
<thead>
<tr>
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>

<tbody>
<tr>
<td class='required'>Project</td>
<td>The name of the project to transform into a plugin</td>
</tr>
<tr>
<td>Plugin</td>
<td>The name of the plugin. If left empty, it will default to
the project name.</td>
</tr>
<tr>
<td>Category</td>
<td>The category of the plugin. It helps filter the plugin list in
the Plugins page and in the picker list.</td>
</tr>
<tr>
<td  class='required'>Version</td>
<td>Version of the plugin to create. A build number is automatically
added (after incrementation) to ensure uniq version numbers.</td>
</tr>
<tr>
<td>Description</td>
<td>The description of the plugin to display in the main
plugin page. If left empty it will default to the
description of the project.</td>
</tr>
<tr>
<td>Author</td>
<td>Name of he author of the project to transform into a plugin.</td>
</tr>
<tr>
<td>Email</td>
<td>The email of the author.</td>
</tr>
<tr>
<td>Comment</td>
<td>if this parameter is not empty, it will be added to the CHANGELOG for traceabiity.</td>
</tr>
<tr>
<td>Directory</td>
<td>The name of the directory where to create the complete file structure. Typically a directory you will check in in your SCM.</td>
</tr>
<tr>
<td>SDK resource</td>
<td>The name of an agent where the Commander SDK is installed. Required to build the plugin.</td>
</tr>
<tr>
<td>SDK Location</td>
<td>The path where the Commander SDK is installed on the above resources..</td>
</tr>
<tr>
<td>fixSelfReferences</td>
<td>Replace self references to the project in sub-prceodure calls by an empty string to be sure the plugins= created will not call the original project instead of itself.</td>
</tr>
<tr>
<td>overwrite</td>
<td>if the ec_setup.pl already exists, it is not overwritten. Check this parameter overwrite with a content dynamically generated based on project properties.</td>
</tr>
<tr>
<td>disableCleanup</td>
<td>Prevents the exported project file to be deleted. It can be an issue when  the agent and server run as different users.</td>
</tr>
<tr>
<td>projectAsCode</td>
<td>Include a bit of code to revert the plugin to a
project upon promotion if this option is checked.</td>
</tr>
<tr>
<td>createArtifact</td>
<td>Save the new created plugin as an artifact version.</td>
</tr>
<tr>
<td>installPlugin</td>
<td>A Boolean to install or not the plugin after creation</td>
</tr>
<tr>
<td>promotePlugin</td>
<td>A Boolean to promote or not the plugin after installation</td>
</tr>
<tr>
<td>moveJobs</td>
<td>A Boolean to move or not jobs from the previous
version into the current one upon promotion.</td>
</tr>
</tbody>
</table>

<img src="src/htdocs/images/help/projectAsCode.png"
alt="projectAsCode parameters" />

<p>At the project level, you can define the addtional following properties to alter the behavior of the plugin creation:</p>
<dl>
<dt><b>configureCredentials:</b></dt>
<dd>this property will indicate to EC-Admin to code to help create configuration to manage credentials for your plugin. It adds:
<ul>
<li>A procedure name "createConfiguration". You need to modify the code of the step "createAndAttachCredential" in order to reference your own procedures and steps where you want to attach those credentials.</li>
<li>Some cgi-bin scripts.</li>
<li>Some java code to manage the different UI to create, edit and delete configurations.</li>
<li>A "configure" link for your plugin on the pluginManager page.</li>
</ul>
Once the procedures and steps have been created, they will not be overwritten so it's safe to modify them to acoomodate your own needs.
</dd>

<dt><b>filesToCopy:</b></dt>
<dd>This property will indicate to the ant plugin builder to add files and directories to your jar file. It will create an entry for the filesToCopy.extras in your build.xml. For example, EC-Admin includes lib/*.jar that is used to manage the configurations required for the credentials.</dd>
<dt><b>pluginDependencies:</b></dt>
<dd>This property allows you to add plugin dependencies that need to be satisfied before you ca install the plugin. Put a a line per dependency with each line being PLUGIN:VERSION</dd>
</dl>

<p><b>Note:</b> EC-Admin is released on
<a href="https://github.com/electriccommunity/EC-Admin">GitHub</a> using
this procedure.</p>

<h1><a name="artifacts"></a>Artifact Management</h1>

<h2>artifactRepositorySynchronization</h2>

<p>Synchronize the content of a source artifact repositories to a target
one. You can use pattern matching to select specific artifact
versions to synchronize.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>Source Artifact Repository</td>
<td>The name of the artifact repository from which to synchronize.</td>
</tr>
<tr>
<td class='required'>Target Artifact Repository</td>
<td>The name of the artifact repository to which to synchronize.</td>
</tr>
<tr>
<td class='required'>Synchronization Resource</td>
<td>The name of a resource used as a buffer to download/upload the files.</td>
</tr>
<tr>
<td>Artifact Version Pattern</td>
<td>Pattern of artifact version to sync. Examples:<br/>
MyGrp:MyKey:* (all artifact versions with group MyGrp and key MyKey),<br/>
MyGrp:* (all artifact versions with group MyGrp),<br/>
* (all artifact versions. This is the default).</td>
</tr>
<tr>
<td>Batch Size</td>
<td>Number of artifact version to process at once. A high number
could overwhelm your gateways.</td>
</tr>
</tbody>
</table>

<img src="src/htdocs/images/help/artifactRepositorySynchronization.png"
alt="artifactRepositorySynchronization parameters" />

<h2>artifactsCleanup</h2>

<p>A procedure to delete artifact versions older than a specified number
of days. Same as for jobs above, a property allows preventing the deletion. Once the
artifacts have been deleted, artifact repositories and artifact caches are
cleaned as well (deleting stale artifacts).  A report mode prevents any
real deletion.</p>

<h2>artifactsCleanup_byQuantity</h2>

<p>As "ArtifactsCleanup"", this procedure deletes artifact versions
older than a specified number
of days but keeps only X per Artifact. Again, a property allows preventing the
deletion. Once the
artifacts have been deleted, artifact repositories and artifact caches are
cleaned as well (deleting stale artifacts).  A report only mode prevents any
real deletion.</p>

<h1><a name="backup"></a>Object export and backup</h1>

<p>This set of procedures is to help you export objects from
ElectricCommander for a potential inclusion into your SCM for
versioning.</p>

<h2>saveProjects</h2>

<p>This procedure simply exports your projects in a directory on the
server. It is recommended to enable the nightly schedule associated
with it to backup your projects on a regular basis.</p>

<h2>saveAllObjects</h2>

<p>This procedure brings a finer granularity to a full server export.
For  example, it may be easier to retrieve a simple project or procedure
instead of the full server export or database backup to undo r a wrong action
or a bad code change. By  default it exports projects and each procedure
individually. In addition by  enabling the matching checkboxes, you can also
export resources,  resource pools, workspaces, users and groups, and with 5.x
and later ElectricFlow Deploy objects (Applications, Components and
Environments). It is recommended to enable the nightly schedule associated
with it to backup your projects on a regular basis.</p>

<p><b>Note:</b> Use saveProjects or saveAllObjects depending on the
granularity you are requiring.</p>

<h2>restoreObjects</h2>
<p>This procedure allows you to import all the .xml files in a directory.
Typically those have been created with saveAllObjects above.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>directory</td>
<td>The directory where the .xml files are located.</td>
</tr>
<tr>
<td>force</td>
<td>A boolean to pass the --force 1 option to import.</td>
</tr>
<tr>
<td>resource</td>
<td>The resource on which to read the files.</td>
</tr>
</tbody>
</table>

<h1><a name="schedules"></a>Schedules</h1>
<p>This set of procedures is to help around schedule enabling and disabling which is very useful when you want to perform maintenace on your server.</p>

<h2>schedulesDisable</h2>
<p>This procedure disables all the enabled schedules and optionaly saves the list in a property.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>Disable</td>
<td>If this option is not set, only report is done. No schedules are disabled.</td>
</tr>
<tr>
<td>Schedule list property</td>
<td>Property path used by the step to save the list of schedules disabled. Can be used by schedulesEnable.</td>
</tr>
</tbody>
</table>

<img src="src/htdocs/images/help/schedulesDisableParameters.png"
alt="schedulesDisable parameters" />

<h2>schedulesEnable</h2>
<p>This procedure enables schedules passed as a list in a property, typically saved by schedulesDisable.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>Enable</td>
<td>If this option is not set, only report is done. No schedules are enabled.</td>
</tr>
<tr>
<td class='required'>Schedule list property</td>
<td>Property path containing the list of disabled schedules.</td>
</tr>
</tbody>
</table>

<img src="src/htdocs/images/help/schedulesEnableParameters.png"
alt="schedulesEnable parameters" />

<h1><a name="semaphore"></a>Semaphore Management</h1>

<h2>acquireSemaphore and releaseSemaphore:</h2>

<p>Those 2 procedures are used to create a "semaphore" to control the access to
a set of steps. In order to insure the atomicity (only one step can modify
your sempahore property at any time) of the operation, you first need to create
a resource with a 1-step limit.<br/>
Then you will need to create a property with a value of 0. The location
does not matter
as long as it will be readable and writable by the plugin.</p>

<h2>acquireSemaphore</h2>
<p>This procedure is the "gate". It checks than you have not used more
tokens than allowed. If you have, it will simly wait until some become
available. This procedure is called just before the steps which access
you want to control.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>maxSemaphoreValue</td>
<td>This is the maximum number of token you allow for this semaphore.</td>
</tr>
<tr>
<td class='required'>semaphoreProperty</td>
<td>Property path containing your gate current value.</td>
</tr>
<tr>
<td class='required'>serializationResource</td>
<td>The name of the 1-step resource to access the semaphore property.</td>
</tr>
</tbody>
</table>
<img src="src/htdocs/images/help/acquireSemaphore.png"
alt="acquireSemaphore parameters" />

<h2>releaseSemaphore</h2>
<p>This procedure simply "releases" of of the token for other to consume.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td class='required'>semaphoreProperty</td>
<td>Property path containing your gate current value.</td>
</tr>
<tr>
<td class='required'>serializationResource</td>
<td>The name of the 1-step resource to access the semaphore property.</td>
</tr>
</tbody>
</table>
<img src="src/htdocs/images/help/releaseSemaphore.png"
alt="releaseSemaphore parameters" />

<h1><a name="license"></a>License Logger</h1>
<p>The License Logger mechanism is intended to collect and email various
license-related statistics. It is implemented as a set of schedules and procedures,
along with a set of configuration properties.</p>
<p>The configuration properties are located under /server/EC-Admin/licenseLogger/config.
The property sheet is created upon promotion if it does not already exists.The following
properties can be set up:</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td>resource</td>
<td>The name of the resource on which to run the procedures. Default is "local".</td>
</tr>
<tr>
<td>workspace</td>
<td>The name of the workspace used by the procedures. Default is "default".</td>
</tr>
<tr>
<td>cleanOldJobs</td>
<td>A boolean to remove old jobs so only the last one is kept for each procedure. It keeps your job history cleaner.</td>
</tr>
<tr>
<td class='required'>emailConfig</td>
<td>The name of the mail configuration used to send report.</td>
</tr>
<tr>
<td class='required'>emailTo</td>
<td>The list of recipients (semicolon separated) to which to send the reports.</td>
</tr>
</tbody>
</table>

<h1><a name="postp"></a>postp debugger helper</h1>
<p>The debugPostp is a simple procedure to help you debug with postp. Traditionally you would call postp from the command line with the jobStepId of the faulty postp and adding some additional option to see details. This new procedure does that for you on Linux and Windows, you just have to pass the jobStepId as a parameter.</p>

<h1><a name="comm"></a>Communication</h1>
<p>The sendAlert is a simple procedure to send a message to all registered users with an email address attached to the account. You can format your email in HTML by encapsulating it in a &lt;html&gt; tag.</p>

<h1><a name="misc"></a>Miscellaneous</h1>
<p>For all those procedure I'm not sure where to put</p>
<h2>findJobId</h2>
<p>This procedure is designed for people migratimg away from ElectricCommander
4.2 to ElectricFlow 5.x or later. It will simply search in the step commands
or parameters to a sub-procedure for any instance of jobId. No change will
be done. It's up to the user to make any change.</p>

<table class="grid">
<thead><tr><th>Parameter</th><th>Description</th></tr></thead>
<tbody>
<tr>
<td>projectPattern</td>
<td>A SQL pattern to filter projects for the search.</td>
</tr>
</tbody>
</table>


<h1>Contact authors</h1>
<dl>
<dt>License Logger</dt>
<dd>Mike Westerhof</dd>

<dt>deleteObjects</dt>
<dt>testResources</dt>
<dd><a href="https://github.com/tanaynagjee">Tanay Nagjee</a></dd>

<dt>Other</dt>
<dd>Laurent Rochette (<a href="mailto:lrochette@electric-cloud.com">lrochette@electric-cloud.com</a>)</dd>
</dl>

<h1>Legal Jumbo</h1>

<p>This module is free for use. Modify it however you see fit to better your
experience using ElectricCommander. Share your enhancements and fixes.</p>

<p>This module is not officially supported by Electric Cloud. It has undergone no
formal testing and you may run into issues that have not been uncovered in the
limited manual testing done so far.</p>

<p>Electric Cloud should not be held liable for any repercussions of using this
software.</p>
