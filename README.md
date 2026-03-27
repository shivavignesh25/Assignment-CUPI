# Assignment-CUPI-
package AutomationAssignment;

import java.io.File;
import java.time.Duration;
import java.util.List;
import java.util.Set;
import org.apache.commons.io.FileUtils;
import org.openqa.selenium.By;
import org.openqa.selenium.Keys;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

public class AutomationTask {

	public static void main(String[] args) throws Exception {
		ChromeOptions options = new ChromeOptions();
        options.addArguments("--start-maximized");

        WebDriver driver = new ChromeDriver(options);
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(15));

        driver.get("https://www.flipkart.com/");
        
        try {
            WebElement closebtn = wait.until(
                    ExpectedConditions.elementToBeClickable(By.cssSelector("button._2KpZ6l._2doB4z"))
            );
            closebtn.click();
        } catch (Exception e) {
            System.out.println("Login popup not displayed");
        }

        
        WebElement search = wait.until(ExpectedConditions.visibilityOfElementLocated(By.name("q")));
        search.sendKeys("Bluetooth speakers");
        search.sendKeys(Keys.ENTER);
        
        try {
            WebElement brand = wait.until(
                    ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='Brand']"))
            );
            brand.click();

            WebElement brand1 = wait.until(
                    ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='boAt']"))
            );
            brand1.click();
        } catch (Exception e) {
            System.out.println("Brand filter not found");
        }

        try {
            WebElement rating = wait.until(
                    ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='4★ & above']"))
            );
            rating.click();
        } catch (Exception e) {
            System.out.println("Rating filter not found");
        }

        try {
            WebElement sort = wait.until(
                    ExpectedConditions.elementToBeClickable(By.xpath("//div[text()='Price -- Low to High']"))
            );
            sort.click();
        } catch (Exception e) {
            System.out.println("Sort option not found");
        }

        Thread.sleep(2000);

        WebElement fp = wait.until(
                ExpectedConditions.elementToBeClickable(By.xpath("(//a[contains(@href,'/p/')])[1]"))
        );

        String parentwindow = driver.getWindowHandle();
        fp.click();

        for (String win : driver.getWindowHandles()) {
            if (!win.equals(parentwindow)) {
                driver.switchTo().window(win);
            }
        }

        Thread.sleep(2000);
        try {
            List<WebElement> offerList = driver.findElements(By.xpath("//li[contains(@class,'_16eBzU')]"));
            System.out.println("Number of offers: " + offerList.size());
        } catch (Exception e) {
            System.out.println("Offers section not found");
        }

        boolean isAvailable = false;
        
        try {
            WebElement addToCart = wait.until(
                    ExpectedConditions.elementToBeClickable(By.xpath("//button[contains(text(),'Add to cart')]"))
            );

            if (addToCart.isDisplayed()) {
                isAvailable = true;
                addToCart.click(); 

                wait.until(ExpectedConditions.urlContains("viewcart"));

                File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
                FileUtils.copyFile(src, new File("./Screenshotimages/cart_result.png"));

                System.out.println("Product added to cart and screenshot saved");
            }
        } catch (Exception e) {
            isAvailable = false;
        }

        if (!isAvailable) {
            System.out.println("Product unavailable");

            File src = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
            FileUtils.copyFile(src, new File("./Screenshotimages/result.png"));
        }

        driver.quit();  
        }
}

