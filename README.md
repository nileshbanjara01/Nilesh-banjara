package LoginpageProject;

import java.util.concurrent.TimeUnit;

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class Logintest 
{	
	WebDriver driver;
    @BeforeMethod
    public void setup() {
		System.setProperty("webdriver.chrome.driver","C:\\selenium webdriver\\chromedriver-win64\\chromedriver.exe");

        driver = new ChromeDriver();
        driver.manage().deleteAllCookies();
        driver.manage().window().maximize();
        driver.get("http://orangehrm.qedgetech.com");
        driver.manage().timeouts().implicitlyWait(20, TimeUnit.SECONDS);
        
    }

    @Test(priority = 1)
    public void validLoginTest() throws InterruptedException {
        // Test scenario: Successful login with valid credentials
         driver.findElement(By.id("txtUsername")).sendKeys("Admin");
         Thread.sleep(2000);
         driver.findElement(By.id("txtPassword")).sendKeys("Qedge123!@#");
         Thread.sleep(2000);
        driver.findElement(By.id("btnLogin")).click();

        

        // Verification - Check if the login was successful
        String expectedURL = "http://orangehrm.qedgetech.com/symfony/web/index.php/dashboard";
        String actualURL = driver.getCurrentUrl();
        Assert.assertEquals(actualURL, expectedURL, "Login Successful");
    }

    @Test(priority = 2)
    public void invalidLoginTest() throws InterruptedException {
        // Test scenario: Login attempt with invalid username/password
    	 driver.findElement(By.id("txtUsername")).sendKeys("Admin");
    	 Thread.sleep(2000);
         driver.findElement(By.id("txtPassword")).sendKeys("nilesh123");
         Thread.sleep(2000);
         driver.findElement(By.id("btnLogin")).click();
        
        // Verification - Check for error message or invalid login handling
       
        WebElement errorMessage = driver.findElement(By.id("spanMessage"));
        Assert.assertTrue(errorMessage.isDisplayed(), "Invalid Login Error Message Displayed");
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}


