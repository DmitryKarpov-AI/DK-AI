# DK-AI
class SmartDevice:
    """Базовый класс для умных устройств"""
    
    def __init__(self, name, power_consumption):
        self.name = name
        self.power_consumption = power_consumption  # в ваттах
        self.is_on = False
    
    def turn_on(self):
        """Включить устройство"""
        if not self.is_on:
            self.is_on = True
            print(f"{self.name}: Устройство включено")
        else:
            print(f"{self.name}: Устройство уже было включено")
    
    def turn_off(self):
        """Выключить устройство"""
        if self.is_on:
            self.is_on = False
            print(f"{self.name}: Устройство выключено")
        else:
            print(f"{self.name}: Устройство уже было выключено")
    
    def get_status(self):
        """Получить статус устройства"""
        status = "включено" if self.is_on else "выключено"
        return f"{self.name}: {status}, потребление: {self.power_consumption} Вт"


class SmartThermostat(SmartDevice):
    """Производный класс - умный термостат"""
    
    def __init__(self, name, power_consumption, current_temp=20, target_temp=22):
        super().__init__(name, power_consumption)
        self.current_temp = current_temp
        self.target_temp = target_temp
    
    def set_temperature(self, temp):
        """Установить целевую температуру"""
        self.target_temp = temp
        print(f"{self.name}: Целевая температура установлена на {temp}°C")
        self._adjust_heating()
    
    def _adjust_heating(self):
        """Внутренний метод для регулировки температуры"""
        if self.is_on:
            if self.current_temp < self.target_temp:
                print(f"{self.name}: Нагрев включен (текущая: {self.current_temp}°C, целевая: {self.target_temp}°C)")
            elif self.current_temp > self.target_temp:
                print(f"{self.name}: Охлаждение включено (текущая: {self.current_temp}°C, целевая: {self.target_temp}°C)")
            else:
                print(f"{self.name}: Температура в норме")
    
    def update_temperature(self, new_temp):
        """Обновить текущую температуру"""
        self.current_temp = new_temp
        print(f"{self.name}: Текущая температура обновлена до {new_temp}°C")
        self._adjust_heating()
    
    def get_status(self):
        """Переопределенный метод для получения статуса"""
        base_status = super().get_status()
        return f"{base_status}, температура: {self.current_temp}°C (цель: {self.target_temp}°C)"


class SmartLight(SmartDevice):
    """Производный класс - умный светильник"""
    
    def __init__(self, name, power_consumption, brightness=50):
        super().__init__(name, power_consumption)
        self.brightness = brightness  # в процентах
    
    def set_brightness(self, level):
        """Установить яркость"""
        if 0 <= level <= 100:
            self.brightness = level
            print(f"{self.name}: Яркость установлена на {level}%")
        else:
            print(f"{self.name}: Недопустимый уровень яркости (должен быть 0-100%)")
    
    def get_status(self):
        """Переопределенный метод для получения статуса"""
        base_status = super().get_status()
        return f"{base_status}, яркость: {self.brightness}%"


# Демонстрация работы классов
if __name__ == "__main__":
    print("=== Демонстрация работы базового и производных классов ===")
    
    # Создаем экземпляры устройств
    basic_device = SmartDevice("Базовое устройство", 50)
    thermostat = SmartThermostat("Умный термостат", 30, 18, 21)
    light = SmartLight("Умный светильник", 15, 75)
    
    # Демонстрация базового класса
    print("\n--- Работа с базовым классом ---")
    print(basic_device.get_status())
    basic_device.turn_on()
    print(basic_device.get_status())
    basic_device.turn_off()
    
    # Демонстрация термостата
    print("\n--- Работа с умным термостатом ---")
    print(thermostat.get_status())
    thermostat.turn_on()
    thermostat.set_temperature(23)
    thermostat.update_temperature(20)
    thermostat.update_temperature(23)
    print(thermostat.get_status())
    
    # Демонстрация светильника
    print("\n--- Работа с умным светильником ---")
    print(light.get_status())
    light.turn_on()
    light.set_brightness(30)
    light.set_brightness(120)  # Некорректное значение
    print(light.get_status())
    light.turn_off()
