# Smoke Tests

Below is a set of tests that exercises each part of the DC21 infrastructure. This should be run after a release to ensure that all parts of the system are running correctly.

This is the minimum set of tests needed. After any release you may want to run some additional tests to exercise new/modified functionality.

<table>
<tr>
<th>Test Name</th>
<th>Test Purpose</th>
<th>Pre- conditions</th>
<th>Test      Steps</th>
<th>Expected Results</th>
</tr>
<td>DC21-Smoke 1</td>
<td>Assure emails can be sent from DC21.</td>
<td>Tester has a valid login to the system & can access the users email account.</td>
<td>1. Go to the home page (ensure your are not already logged in).<br>
2. Click “Forgot your password?” button.<br>
3. Enter your user’s email address & click “Send me reset password”.<br>
</td>
<td>1. User should receive an email with a link to reset their password.<br>
2. User can click the link in the email and is taken to the change your password screen<br/>
Note: for the purposes of this test you do not need to actually reset your password, we just want to check that emails can be sent.
</td>
</tr>
<tr>
<td>DC21-Smoke 2</td>
<td>Assure DC21 can access the MINT to retrieve FOR codes.</td>
<td>1. You are logged in as a Researcher.<br/>
2. You have some facilities and experiments set up.</td>
<td>
1. Click "Facilities" tab<br/>
2. Click on an existing facility<br/>
3. Click on an existing experiment<br/>
4. Click 'Edit Experiment'
</td>
<td>
1. The FOR codes dropdown is populated.<br/>
2. The second and third level FOR code dropdowns are populated after the previous one is selected.
</tr>
<tr>
<td>DC21-Smoke 3</td>
<td>Assure files can be uploaded to DC21.</td>
<td>You are logged in as a Researcher.</td>
<td>
1. Click "Upload"<br/>
2. Select a file type, experiment, tags and enter a description<br/>
3. Select a suitable file to upload<br/>
4. Click Upload
</td>
<td>
1. File is uploaded successfully and can be found via search<br>
Note: you may wish to delete the file afterwards.
</td>
</tr>
<tr>
<td>DC21-Smoke 4</td>
<td>Assure files can be uploaded through the API.</td>
<td>1. PC upload scripts are successfully installed as per [PC Upload Setup](Setting_Up_Automated_Load_From_PC.md)<br>
2. User has generated an authentication token.<br>
3. A valid config file has been generated for the PC upload scripts, and is configured to upload a suitable sample file for testing.
</td>
<td>
1. Run the PC load batch file to upload the file.
</td>
<td>
1. The PC load log file has an entry with details about the file uploaded and a success message.<br>
2. After logging into DC21, you can find the newly uploaded file via search.<br/>
3. The file has the correct metadata and can be downloaded from DC21.<br/>
</td>
</tr>
<tr>
<td>DC21-Smoke 5</td>
<td>Assure files can be downloaded from DC21.</td>
<td>You are logged in as a Researcher.</td>
<td>
1. Go to Explore Data tab.<br/>
2. Search by multiple filters.</br>
3. After a successful search, add one file and click "Download"<br/>
4. Then add multiple files and click "Download"<br/>
</td>
<td>
1. Searches find the correct files.<br/>
2. The selected files can be downloaded. The download zip includes the correct files as well as metadata files relevant to the selected files. Check that file, facility and experiment metadata matches with what is displayed in the web application.
</td>
</tr>
<tr>
<td>DC21-Smoke 6</td>
<td>Assure OAI-PMH Feed</td>
<td>Logged is an an Administrator and User has created a Package that is not yet published (but is process-complete).</td>
<td>1. Go to Package detail page and note PackageID in the url<br>
2. On a terminal<br>
$ ssh devel@(your DC21 Instance url) e.g. ssh devel@jp-dc21-staging.intersect.org.au <br>
$ (enter your devel password) <br>
$ cd /data/dc21-data/unpublished_rif_cs <br>
$ ls (a list of unpublished rif-cs xmlfiles should display, including "rif-cs-(yourpackageID).xml" <br>
3. In DC21 instance, select "Publish" (User should see a success message) <br>
4. In the terminal <br>
$ ls    (your "rif-cs-(yourpackageID)" should no longer display) <br>
$ cd .. <br>
$ cd published_rif_cs <br>
Your "rif-cs-(yourpackageID).xml" should now display in this list.<br>
 </td>
<td>
There are "rif-cs-<ID>.xml" elements present for each published collection.<br>
</tr>
<tr>
<td>DC21-Smoke 7</td>
<td>Assure that SSL is correctly configured</td>
<td></td>
<td>
1. Go to https://< DC21-server-url >where < DC21-server-url > is your production server<br>
2. Go to http://< DC21-server-url > where < DC21-server-url > is your production server<br>
</td>
<td>
1. When visiting https://< DC21-server-url >, the home page is displayed. Browser displays padlock or similar to indicate that you are on a secure connection.<br>
2. When visiting http://< DC21-server-url >, you are redirected from http onto https<br>
</tr>
<tr>
<td>DC21-Smoke 8</td>
<td>Assure that AAF is correctly configured</td>
<td></td>
<td>
1. Go to https://< DC21-server-url >where < DC21-server-url > is your production server<br>
2. Click "Log in via AAF"<br>
</td>
<td>
1. You should be redirected to AAF to choose your Identity Provider and etc
</td>
</tr>
<tr>
<td>DC21-Smoke 9</td>
<td>Assure that Tesseract OCR is working</td>
<td>
1. Your ABBYY details have been left blank in the Admin interface<br>
</td>
<td>1. Upload a .jpg file (eg image.jpg)<br>
2. Go to the image.jpg data file details page<br>
3. Press the OCR button
</td>
<td>
1. The file image.jpg.txt should be created as the child of image.jpg<br>
2. When processing is complete, image.jpg.txt should have "This file was automatically generated by OCR (Tesseract ...)" in the descripton.
</td>
</tr>
<tr>
<td>DC21-Smoke 10</td>
<td>Assure that ABBYY OCR is working</td>
<td>1. You must have an account set up with an ABBYY instance.<br>
2. You must have the account details set up with the Admin interface.
</td>
<td>1. Upload a .jpg file (eg image.jpg)<br>
2. Go to the image.jpg data file details page<br>
3. Press the OCR button
</td>
<td>1. The file image.jpg.txt should be created as the child of image.jpg<br>
2. When processing is complete, image.jpg.txt should have "This file was automatically generated by OCR (ABBYY ...)" in the descripton.
</td>
</tr>
<tr>
<td>DC21-Smoke 11</td>
<td>Assure that SR is working</td>
<td>1. You must have an account set up with Koemei.<br>
2. You must have the account details set up with the Admin interface.
</td>
<td>1. Upload a .mp3 file (eg audio.mp3)<br>
2. Go to the audio.mp3 data file details page<br>
3. Press the SR button
</td>
<td>1. The file audio.mp3.txt should be created as the child of audio.mp3<br>
2. When processing is complete, audio.mp3.txt should have "This file was automatically generated by SR (Koemei)" in the descripton.
</td>
</tr>
<tr>
<td>DC21-Smoke 12</td>
<td>Assure that netcdf files can be uploaded</td>
<td>You are logged in as a Researcher. </td>
<td>
1. Click "Upload"<br/>
2. Select a file type, experiment, tags and enter a description<br/>
3. Select a .nc file to upload<br/>
4. Click Upload
</td>
<td>
1. File is uploaded successfully and can be found via search<br>
2. Click on the Filename (in the explore data tab) it should have file format at NETCDF, Information from the File and Variables (5 columns : Column, Column Mapping, Unit, Measurement Type, Fill Value)<br>
Note: you may wish to delete the file afterwards.
</td>
</tr>
<tr>
<td>DC21-Smoke 13</td>
<td> Check once a package is published check that the jOAI system receives the package and is ready to upstream it</td>
<td>You are logged in as a Researcher and already published a package. You also have SSH access to the VM.</td>
<td>
1.  Once a package appears in the /published_rif_cs/ folder grab the Package ID from the filename eg. "rif-cs-86752.xml, so 86752.<br/>
</td>
<td>
The package should be in this response from the jOAI provider - http://<hostname>/oai/provider?verb=ListIdentifiers&metadataPrefix=rif <br/>
And http://<hostname>/oai/provider?verb=GetRecord&metadataPrefix=rif&identifier=oai:hiev.uws.edu.au:rif-cs-86752
should show the relevant file
</td>
</tr>
<table>
