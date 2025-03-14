# Erajaya
## framework JUnit
<dependencies>
    <dependency>
        <groupId>io.appium</groupId>
        <artifactId>java-client</artifactId>
        <version>8.4.1</version>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-api</artifactId>
        <version>5.7.0</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.7.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>

## Set Environment

import io.appium.java_client.MobileElement;
import io.appium.java_client.android.AndroidDriver;
import io.appium.java_client.remote.MobileCapabilityType;
import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.remote.DesiredCapabilities;
import java.net.URL;

@BeforeAll
public static void setUp() throws Exception {
    DesiredCapabilities caps = new DesiredCapabilities();
    caps.setCapability(MobileCapabilityType.PLATFORM_NAME, "Android"); // Menentukan platform Android
    caps.setCapability(MobileCapabilityType.PLATFORM_VERSION, "11");  // Versi Android
    caps.setCapability(MobileCapabilityType.DEVICE_NAME, "Pixel_3a"); // Nama perangkat/emulator
    caps.setCapability(MobileCapabilityType.APP_PACKAGE, "com.eraspace"); // Nama paket aplikasi Eraspace
    caps.setCapability(MobileCapabilityType.APP_ACTIVITY, "com.eraspace.MainActivity"); // Aktivitas utama aplikasi
    caps.setCapability(MobileCapabilityType.NO_RESET, true); // Tidak mereset aplikasi antara tes

    // Inisialisasi driver untuk menghubungkan dengan Appium server
    URL url = new URL("http://127.0.0.1:4723/wd/hub"); // URL server Appium
    driver = new AndroidDriver<>(url, caps); // Membuat driver untuk menjalankan tes di Android
}


## Test case 1.1

@Test
public void loginWithInvalidCredentials() {
    // Mencari elemen untuk input username, password, dan tombol login
    MobileElement usernameField = driver.findElement(By.id("com.eraspace:id/username"));
    MobileElement passwordField = driver.findElement(By.id("com.eraspace:id/password"));
    MobileElement loginButton = driver.findElement(By.id("com.eraspace:id/login_button"));
    
    // Menginput data yang tidak valid
    usernameField.sendKeys("invalid_user"); // Username salah
    passwordField.sendKeys("invalid_password"); // Password salah
    
    // Klik tombol login
    loginButton.click();
    
    // Validasi pesan error yang muncul
    MobileElement errorMessage = driver.findElement(By.id("com.eraspace:id/error_message"));
    assert(errorMessage.isDisplayed()); // Pastikan pesan error muncul
    assert(errorMessage.getText().contains("Login failed")); // Memastikan bahwa pesan error berisi "Login failed"
}

## Test case 1.2

@Test
public void loginWithValidCredentials() {
    // Mencari elemen untuk input username, password, dan tombol login
    MobileElement usernameField = driver.findElement(By.id("com.eraspace:id/username"));
    MobileElement passwordField = driver.findElement(By.id("com.eraspace:id/password"));
    MobileElement loginButton = driver.findElement(By.id("com.eraspace:id/login_button"));
    
    // Menginput data yang valid
    usernameField.sendKeys("valid_user"); // Username valid
    passwordField.sendKeys("valid_password"); // Password valid
    
    // Klik tombol login
    loginButton.click();
    
    // Validasi setelah login sukses
    MobileElement homepageMenu = driver.findElement(By.id("com.eraspace:id/menu"));
    MobileElement accountName = driver.findElement(By.id("com.eraspace:id/account_name"));
    MobileElement points = driver.findElement(By.id("com.eraspace:id/points"));
    
    // Memastikan bahwa homepage berhasil ditampilkan
    assert(homepageMenu.isDisplayed()); // Menu homepage muncul
    assert(accountName.isDisplayed()); // Nama akun muncul
    assert(points.isDisplayed()); // Poin pengguna muncul
    
    // Verifikasi apakah aplikasi mengarahkan ke homepage
    assert(driver.getCurrentUrl().contains("homepage")); // Memastikan aplikasi mengarahkan ke homepage
}

