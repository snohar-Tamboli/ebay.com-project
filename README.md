# ebay.com-project
import org.junit.jupiter.api.*;
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.*;
import org.openqa.selenium.support.ui.*;
import io.github.bonigarcia.wdm.WebDriverManager;

import static org.junit.jupiter.api.Assertions.*;

public class eBayAddToCartTest {

    private WebDriver driver;
    private WebDriverWait wait;

    @BeforeEach
    public void setUp() {
        // Set up the WebDriver (automatically manages the correct driver version)
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        wait = new WebDriverWait(driver, 10);  // Wait for elements to be ready
    }

    @Test
    public void testAddItemToCart() {
        // Step 1: Open eBay website
        driver.get("https://www.ebay.com");

        // Step 2: Search for 'book'
        WebElement searchBox = wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("gh-ac")));
        searchBox.sendKeys("book");
        searchBox.submit();

        // Step 3: Click on the first book in the search results
        WebElement firstItem = wait.until(ExpectedConditions.elementToBeClickable(By.cssSelector(".s-item:nth-child(1) .s-item__link")));
        firstItem.click();

        // Step 4: Click on the 'Add to cart' button
        WebElement addToCartButton = wait.until(ExpectedConditions.elementToBeClickable(By.id("atcRedesignId_btn")));
        addToCartButton.click();

        // Step 5: Verify the cart has been updated
        WebElement cartIcon = wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("gh-cart-n")));
        String cartCount = cartIcon.getText();
        
        // Step 6: Assert that the cart count is greater than 0 (indicating the item has been added)
        assertNotNull(cartCount, "Cart count should not be null");
        assertTrue(Integer.parseInt(cartCount) > 0, "Cart should have at least 1 item");
    }

    @AfterEach
    public void tearDown() {
        // Close the browser after each test
        if (driver != null) {
            driver.quit();
        }
    }
}
