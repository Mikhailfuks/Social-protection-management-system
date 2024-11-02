CREATE TABLE Users (
    UserId INT PRIMARY KEY IDENTITY(1,1),
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    BirthDate DATE,
    Address VARCHAR(255),
    PhoneNumber VARCHAR(20),
    Email VARCHAR(255),
    IdentityDocument VARCHAR(20),
    DocumentNumber VARCHAR(20),
    SocialSecurityNumber VARCHAR(20),
    RegistrationDate DATE,
    Status VARCHAR(50) -- Например, "Active", "Inactive"
);

-- Создание таблицы для видов социальной защиты
CREATE TABLE SocialBenefits (
    BenefitId INT PRIMARY KEY IDENTITY(1,1),
    BenefitName VARCHAR(255) NOT NULL,
    Description TEXT,
    EligibilityCriteria TEXT,
    Amount DECIMAL(10,2),
    PaymentFrequency VARCHAR(50) -- Например, "Monthly", "Quarterly"
);

-- Создание таблицы для заявлений на социальные пособия
CREATE TABLE BenefitApplications (
    ApplicationId INT PRIMARY KEY IDENTITY(1,1),
    UserId INT NOT NULL,
    BenefitId INT NOT NULL,
    ApplicationDate DATE,
    Status VARCHAR(50) -- Например, "Pending", "Approved", "Rejected"
    FOREIGN KEY (UserId) REFERENCES Users(UserId),
    FOREIGN KEY (BenefitId) REFERENCES SocialBenefits(BenefitId)
);

-- Создание таблицы для выплат
CREATE TABLE Payments (
    PaymentId INT PRIMARY KEY IDENTITY(1,1),
    ApplicationId INT NOT NULL,
    PaymentDate DATE,
    Amount DECIMAL(10,2),
    Status VARCHAR(50) -- Например, "Pending", "Processed"
    FOREIGN KEY (ApplicationId) REFERENCES BenefitApplications(ApplicationId)
);

-- Создание таблицы для истории изменений статуса заявлений
CREATE TABLE ApplicationStatusHistory (
    HistoryId INT PRIMARY KEY IDENTITY(1,1),
    ApplicationId INT NOT NULL,
    StatusChangeDate DATE,
    OldStatus VARCHAR(50),
    NewStatus VARCHAR(50),
    Reason TEXT,
    FOREIGN KEY (ApplicationId) REFERENCES BenefitApplications(ApplicationId)
);

-- Пример добавления пользователя
INSERT INTO Users (FirstName, LastName, BirthDate, Address, PhoneNumber, Email, IdentityDocument, DocumentNumber, SocialSecurityNumber, RegistrationDate, Status) VALUES
('Иван', 'Иванов', '1980-01-01', 'ул. Ленина, д. 1', '+79001234567', 'ivan.ivanov@mail.ru', 'Паспорт', '1234567890', '123456789012', '2023-03-15', 'Active');

-- Пример добавления вида социальной защиты
INSERT INTO SocialBenefits (BenefitName, Description, EligibilityCriteria, Amount, PaymentFrequency) VALUES
('Пособие по безработице', 'Ежемесячная выплата для безработных граждан', 'Отсутствие работы и наличие статуса безработного', 10000, 'Monthly');

-- Пример добавления заявления на пособие
INSERT INTO BenefitApplications (UserId, BenefitId, ApplicationDate, Status) VALUES
(1, 1, '2023-03-16', 'Pending');

-- Пример добавления выплаты
INSERT INTO Payments (ApplicationId, PaymentDate, Amount, Status) VALUES
(1, '2023-03-20', 10000, 'Processed');

-- Пример добавления записи в историю изменений статуса заявления
INSERT INTO ApplicationStatusHistory (ApplicationId, StatusChangeDate, OldStatus, NewStatus, Reason) VALUES
(1, '2023-03-17', 'Pending', 'Approved', 'Заявление одобрено после проверки');
