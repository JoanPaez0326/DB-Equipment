# DB-Equipment
Base de datos 
-- phpMyAdmin SQL Dump
-- version 5.2.1
-- https://www.phpmyadmin.net/
--
-- Servidor: 127.0.0.1
-- Tiempo de generación: 23-10-2025 a las 02:44:29
-- Versión del servidor: 10.4.32-MariaDB
-- Versión de PHP: 8.2.12

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Base de datos: `equipment_loans`
--

-- --------------------------------------------------------

--
-- Estructura de tabla para la tabla `devices`
--

CREATE TABLE `devices` (
  `id` int(11) NOT NULL,
  `device_number` varchar(50) NOT NULL,
  `type` enum('PC','Videobeam','Impresora') NOT NULL,
  `brand` varchar(100) NOT NULL,
  `description` text DEFAULT NULL,
  `with_charger` tinyint(1) DEFAULT 1,
  `status` enum('Disponible','Prestado','En Mantenimiento','Dañado') DEFAULT 'Disponible',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

-- --------------------------------------------------------

--
-- Estructura de tabla para la tabla `equipment`
--

CREATE TABLE `equipment` (
  `id` int(11) NOT NULL,
  `name` varchar(100) DEFAULT NULL,
  `type` enum('Portátil','Impresora','Video Beam','Disco Duro','Mouse','Otro') DEFAULT NULL,
  `brand` varchar(100) DEFAULT NULL,
  `serial_number` varchar(100) DEFAULT NULL,
  `description` text DEFAULT NULL,
  `status` enum('Disponible','Prestado','En Mantenimiento') DEFAULT 'Disponible',
  `quantity` int(11) DEFAULT 1,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Volcado de datos para la tabla `equipment`
--

INSERT INTO `equipment` (`id`, `name`, `type`, `brand`, `serial_number`, `description`, `status`, `quantity`, `created_at`) VALUES
(1, 'pc', 'Portátil', 'dell', '', 'Buen estado', 'Disponible', 1, '2025-04-10 22:52:42'),
(2, 'video beam', 'Portátil', 'hp', NULL, NULL, 'Disponible', 1, '2025-04-11 05:36:36'),
(3, 'portatil', 'Portátil', 'dell', NULL, NULL, 'Disponible', 0, '2025-04-11 08:03:43');

-- --------------------------------------------------------

--
-- Estructura de tabla para la tabla `loans`
--

CREATE TABLE `loans` (
  `id` int(11) NOT NULL,
  `user_id` int(11) NOT NULL,
  `equipment_id` int(10) UNSIGNED DEFAULT NULL,
  `loan_date` date NOT NULL,
  `loan_time` time NOT NULL,
  `return_time` time DEFAULT NULL,
  `due_date` date DEFAULT NULL,
  `role` enum('student','professor','admin') NOT NULL,
  `status` enum('Activo','Finalizado','Vencido') DEFAULT 'Activo'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Volcado de datos para la tabla `loans`
--

INSERT INTO `loans` (`id`, `user_id`, `equipment_id`, `loan_date`, `loan_time`, `return_time`, `due_date`, `role`, `status`) VALUES
(1, 13, NULL, '2025-10-15', '19:50:21', NULL, '2025-10-15', 'student', 'Activo'),
(2, 13, 3, '2025-10-15', '20:33:48', NULL, '2025-10-15', 'student', 'Activo');

-- --------------------------------------------------------

--
-- Estructura de tabla para la tabla `users`
--

CREATE TABLE `users` (
  `id` int(11) NOT NULL,
  `name` varchar(100) NOT NULL,
  `email` varchar(100) NOT NULL,
  `identity_document` varchar(20) DEFAULT NULL,
  `password` varchar(255) NOT NULL,
  `role` enum('admin','teacher','student') DEFAULT 'student',
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NOT NULL DEFAULT current_timestamp() ON UPDATE current_timestamp(),
  `phone` varchar(20) DEFAULT NULL,
  `program` varchar(150) DEFAULT NULL,
  `schedule` enum('Jornada Diurna','Jornada Nocturna','Jornada Sabática') DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Volcado de datos para la tabla `users`
--

INSERT INTO `users` (`id`, `name`, `email`, `identity_document`, `password`, `role`, `created_at`, `updated_at`, `phone`, `program`, `schedule`) VALUES
(9, 'Administrador Del Sistema', 'admin@utede.edu.co', '123456789', '$2y$10$rLyf0aIwOpoFdMbsxHMgfexTdtdZ2XvzNd36k9pNJ.KAQmSuRpIi6', 'admin', '2025-04-10 22:40:43', '2025-04-10 22:40:43', NULL, NULL, NULL),
(12, 'JOAN SEBASTIAN PAEZ MURILLO', 'sebastiannpaez13@gmail.com', '1006230120', '$2y$10$RLzJk60aw3l8ZT5fnJTtBOlx6K5gbw4nh02mkLRJuOJ.iFuSsuQT2', 'admin', '2025-10-02 00:18:11', '2025-10-02 00:18:11', NULL, NULL, NULL),
(13, 'Anderson Ospina', 'ander@gmail.com', '1006017573', '$2y$10$Iyt5I1mdQFrwSp5qhGwjU.C/dntaHvTh1McHa3/JpVvrK4wl1rMmq', 'student', '2025-10-02 00:43:08', '2025-10-02 00:43:08', '3214562145', 'Técnico Profesional en Programación de Software', 'Jornada Nocturna');

--
-- Índices para tablas volcadas
--

--
-- Indices de la tabla `devices`
--
ALTER TABLE `devices`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `device_number` (`device_number`);

--
-- Indices de la tabla `equipment`
--
ALTER TABLE `equipment`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `serial_number` (`serial_number`);

--
-- Indices de la tabla `loans`
--
ALTER TABLE `loans`
  ADD PRIMARY KEY (`id`),
  ADD KEY `user_id` (`user_id`),
  ADD KEY `idx_loans_equipment_id` (`equipment_id`);

--
-- Indices de la tabla `users`
--
ALTER TABLE `users`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `email` (`email`),
  ADD UNIQUE KEY `identity_document` (`identity_document`);

--
-- AUTO_INCREMENT de las tablas volcadas
--

--
-- AUTO_INCREMENT de la tabla `devices`
--
ALTER TABLE `devices`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;

--
-- AUTO_INCREMENT de la tabla `equipment`
--
ALTER TABLE `equipment`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=4;

--
-- AUTO_INCREMENT de la tabla `loans`
--
ALTER TABLE `loans`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=3;

--
-- AUTO_INCREMENT de la tabla `users`
--
ALTER TABLE `users`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=14;

--
-- Restricciones para tablas volcadas
--

--
-- Filtros para la tabla `loans`
--
ALTER TABLE `loans`
  ADD CONSTRAINT `loans_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`);
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
