<h3>Summary</h3>
Goodbye to folder for storing screenshot. Everytime attaching the screenshot folder to clients reports, its seem big task and big storage. To reduce these, we using <h4>Base64screenshot</h4> method to attach screenshot in extent report html file.

<h3>Expected Behavior</h3>
Ideally the screenshot after attached in extent must be same as that of the original screenshot

<h3>Current Behavior</h3>
Storing screenshot in project folder.

<h3>Sample</h3>
package featureLibrary.nativeApps;

import base.GenericWrappers;
import com.cucumber.listener.Reporter;
import cucumber.api.Scenario;
import org.apache.commons.codec.binary.Base64;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.remote.RemoteWebDriver;

import java.io.File;
import java.io.FileInputStream;
import java.util.logging.Logger;

public class ScreenShot {

    public static RemoteWebDriver driver;
    public void takeScreenShotonFailure(Scenario scenario) {
        String encodedBase64 = null;
        Logger logger = Logger.getLogger(String.valueOf(GenericWrappers.class));
        FileInputStream fileInputStreamReader = null;
        if (scenario.getStatus().equalsIgnoreCase("failed")) {
            try {
                File scrFile = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
                fileInputStreamReader = new FileInputStream(scrFile);
                byte[] bytes = new byte[(int) scrFile.length()];
                fileInputStreamReader.read(bytes);
                encodedBase64 = new String(Base64.encodeBase64(bytes));
                scenario.embed(bytes, "image/png");
                Reporter.addScreenCaptureFromPath("data:image/png;base64," + encodedBase64);
            } catch (Exception e) {
                e.printStackTrace();
            }
            logger.info("Screen Shot taken");
        }
    }
}

<h3>Environment Details</h3>
Extent report Version used: 4.0.6
Operating System and version: Windows 10, chrome 53
JDK Version: 1.8

  
  
