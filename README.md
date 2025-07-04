# LoadConnect
-- Converted PostgreSQL Schema for Neon Console
-- All tables converted from original MySQL dump

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(15),
    user_type VARCHAR(20) DEFAULT 'shipper',
    company_name VARCHAR(100),
    address TEXT,
    city VARCHAR(50),
    state VARCHAR(50),
    pincode VARCHAR(10),
    is_verified BOOLEAN DEFAULT false,
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE truck_operators (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    email VARCHAR(100),
    mobile VARCHAR(20),
    dob DATE,
    number_of_trucks INTEGER,
    address TEXT,
    country VARCHAR(50),
    pincode VARCHAR(20),
    district VARCHAR(100),
    state VARCHAR(100),
    city VARCHAR(100),
    pan_number VARCHAR(50),
    gst_number VARCHAR(50),
    aadhaar_number VARCHAR(50),
    driving_license_number VARCHAR(50),
    detail_collection TEXT,
    documents TEXT
);

CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    mobile VARCHAR(20),
    dob DATE,
    address TEXT,
    country VARCHAR(50),
    pincode VARCHAR(10),
    district VARCHAR(100),
    state VARCHAR(100),
    city VARCHAR(100),
    pan_number VARCHAR(50),
    gst_number VARCHAR(50),
    aadhaar_number VARCHAR(50),
    detail_collection TEXT,
    documents TEXT
);

CREATE TABLE loads (
    id SERIAL PRIMARY KEY,
    load_type VARCHAR(50),
    source_city VARCHAR(100),
    destination_city VARCHAR(100),
    material_type VARCHAR(100),
    weight NUMERIC(10,2),
    truck_type VARCHAR(100),
    number_of_trucks INTEGER,
    scheduled_date DATE,
    posted_by VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE quotations (
    id SERIAL PRIMARY KEY,
    load_id INTEGER NOT NULL,
    quoted_by INTEGER NOT NULL,
    quoted_price NUMERIC(10,2) NOT NULL,
    message TEXT,
    delivery_time VARCHAR(50),
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (load_id, quoted_by),
    FOREIGN KEY (quoted_by) REFERENCES users(id) ON DELETE CASCADE
);

CREATE TABLE bookings (
    id SERIAL PRIMARY KEY,
    load_id INTEGER NOT NULL,
    quotation_id INTEGER NOT NULL,
    shipper_id INTEGER NOT NULL,
    transporter_id INTEGER NOT NULL,
    booking_amount NUMERIC(10,2) NOT NULL,
    advance_amount NUMERIC(10,2) DEFAULT 0.00,
    pickup_date DATE,
    delivery_date DATE,
    actual_pickup_date DATE,
    actual_delivery_date DATE,
    status VARCHAR(20) DEFAULT 'confirmed',
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (load_id) REFERENCES loads(id) ON DELETE CASCADE,
    FOREIGN KEY (quotation_id) REFERENCES quotations(id) ON DELETE CASCADE,
    FOREIGN KEY (shipper_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (transporter_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE TABLE tracking (
    id SERIAL PRIMARY KEY,
    truck_id VARCHAR(50),
    latitude NUMERIC(10,7),
    longitude NUMERIC(10,7),
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE tracking_logs (
    id SERIAL PRIMARY KEY,
    truck_id VARCHAR(50) NOT NULL,
    latitude DOUBLE PRECISION NOT NULL,
    longitude DOUBLE PRECISION NOT NULL,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE vehicles (
    id SERIAL PRIMARY KEY,
    owner_id INTEGER NOT NULL,
    vehicle_number VARCHAR(20) NOT NULL UNIQUE,
    vehicle_type VARCHAR(50) NOT NULL,
    capacity NUMERIC(8,2) NOT NULL,
    driver_name VARCHAR(100),
    driver_phone VARCHAR(15),
    driver_license VARCHAR(50),
    is_available BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (owner_id) REFERENCES users(id) ON DELETE CASCADE
);
