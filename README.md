import java.util.*;

// Model: Represents the data structures
class Tire {
    private int pressure;

    public Tire(int pressure) {
        this.pressure = pressure;
    }

    public int getPressure() {
        return pressure;
    }
}

class ServiceAlert {
    private String message;

    public ServiceAlert(String message) {
        this.message = message;
    }

    public String getMessage() {
        return message;
    }
}

// View: Handles the display of information
class VehicleView {
    public void displayTirePressure(Tire tire) {
        System.out.println("Tire Pressure: " + tire.getPressure() + " PSI");
    }

    public void displayServiceAlert(ServiceAlert alert) {
        System.out.println("Service Alert: " + alert.getMessage());
    }

    public void displayMessage(String message) {
        System.out.println(message);
    }
}

// Hardware Interface: Defines the contract for hardware operations
interface HardwareInterface {
    void unlockDoors();
    void lockDoors();
    void startEngine();
    void stopEngine();
    int readTirePressure();
    String fetchServiceAlert();
}

// Singleton: Ensures a single instance of the hardware controller
class VehicleHardwareController {
    private static VehicleHardwareController instance;

    private VehicleHardwareController() {
        // Initialize hardware connections
    }

    public static VehicleHardwareController getInstance() {
        if (instance == null) {
            instance = new VehicleHardwareController();
        }
        return instance;
    }

    public void unlockDoors() {
        // Hardware logic to unlock doors
        System.out.println("Hardware: Doors unlocked.");
    }

    public void lockDoors() {
        // Hardware logic to lock doors
        System.out.println("Hardware: Doors locked.");
    }

    public void startEngine() {
        // Hardware logic to start engine
        System.out.println("Hardware: Engine started.");
    }

    public void stopEngine() {
        // Hardware logic to stop engine
        System.out.println("Hardware: Engine stopped.");
    }

    public int readTirePressure() {
        // Hardware logic to read tire pressure
        return 32; // Example pressure
    }

    public String fetchServiceAlert() {
        // Hardware logic to fetch service alerts
        return "Oil change needed.";
    }
}

// Adapter: Adapts the hardware controller to the hardware interface
class HardwareAdapter implements HardwareInterface {
    private VehicleHardwareController controller;

    public HardwareAdapter() {
        this.controller = VehicleHardwareController.getInstance();
    }

    @Override
    public void unlockDoors() {
        controller.unlockDoors();
    }

    @Override
    public void lockDoors() {
        controller.lockDoors();
    }

    @Override
    public void startEngine() {
        controller.startEngine();
    }

    @Override
    public void stopEngine() {
        controller.stopEngine();
    }

    @Override
    public int readTirePressure() {
        return controller.readTirePressure();
    }

    @Override
    public String fetchServiceAlert() {
        return controller.fetchServiceAlert();
    }
}

// Facade: Provides a simplified interface to the hardware operations
class VehicleFacade {
    private HardwareInterface hardware;

    public VehicleFacade() {
        this.hardware = new HardwareAdapter();
    }

    public void unlockDoors() {
        hardware.unlockDoors();
    }

    public void lockDoors() {
        hardware.lockDoors();
    }

    public void startEngine() {
        hardware.startEngine();
    }

    public void stopEngine() {
        hardware.stopEngine();
    }

    public Tire getTirePressure() {
        int pressure = hardware.readTirePressure();
        return new Tire(pressure);
    }

    public ServiceAlert getServiceAlert() {
        String message = hardware.fetchServiceAlert();
        return new ServiceAlert(message);
    }
}

// Controller: Handles user interactions and updates the view
class VehicleController {
    private VehicleFacade facade;
    private VehicleView view;

    public VehicleController(VehicleFacade facade, VehicleView view) {
        this.facade = facade;
        this.view = view;
    }

    public void unlockDoors() {
        facade.unlockDoors();
        view.displayMessage("Doors unlocked.");
    }

    public void lockDoors() {
        facade.lockDoors();
        view.displayMessage("Doors locked.");
    }

    public void startEngine() {
        facade.startEngine();
        view.displayMessage("Engine started.");
    }

    public void stopEngine() {
        facade.stopEngine();
        view.displayMessage("Engine stopped.");
    }

    public void checkTirePressure() {
        Tire tire = facade.getTirePressure();
        view.displayTirePressure(tire);
    }

    public void getServiceAlert() {
        ServiceAlert alert = facade.getServiceAlert();
        view.displayServiceAlert(alert);
    }
}

// Main Application
public class Main {
    public static void main(String[] args) {
        VehicleFacade facade = new VehicleFacade();
        VehicleView view = new VehicleView();
        VehicleController controller = new VehicleController(facade, view);

        controller.unlockDoors();
        controller.startEngine();
        controller.checkTirePressure();
        controller.getServiceAlert();
        controller.stopEngine();
        controller.lockDoors();
    }
}
