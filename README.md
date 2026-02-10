# Методичка по выполнению модуля А (Фронтенд)
## Часть 7: Страница "Личный кабинет" и "Админ-панель"

**Цель:** Создать личный кабинет участника с заявками и админ-панель для управления компетенциями.

### Шаги:

1. **Создание данных для личного кабинета**
   Создайте файл `src/data/userData.js`:

   ```javascript
   // Данные пользователя
   export const userData = {
     id: 1,
     fullName: 'Иванов Александр Сергеевич',
     email: 'alex@example.com',
     category: 'Студент',
     region: 'Москва',
     university: 'МГТУ им. Баумана',
     photo: 'https://via.placeholder.com/150/3498db/ffffff?text=ИА',
     registrationDate: '2026-03-15'
   };

   // Заявки пользователя
   export const userApplications = [
     {
       id: 1,
       competence: 'Веб-разработка',
       status: 'На рассмотрении',
       date: '2026-03-20',
       comment: 'Ожидает подтверждения эксперта',
       documents: ['Задание_веб.pdf', 'Резюме.pdf']
     },
     {
       id: 2,
       competence: 'Мобильная разработка',
       status: 'Одобрена',
       date: '2026-03-18',
       comment: 'Заявка принята, ожидайте инструкции',
       documents: ['Задание_мобильное.pdf']
     },
     {
       id: 3,
       competence: 'Системное администрирование',
       status: 'Отклонена',
       date: '2026-03-10',
       comment: 'Необходимо предоставить дополнительные документы',
       documents: ['Задание_админ.pdf']
     }
   ];

   // Доступные компетенции для подачи заявки
   export const availableCompetencies = [
     'Веб-разработка',
     'Графический дизайн',
     'Мобильная разработка',
     'Кулинарное искусство',
     '3D-моделирование',
     'Флористика',
     'Системное администрирование',
     'Швейное дело'
   ];
   ```

2. **Создание компонента ApplicationCard**
   В папке `src/components/` создайте файл `ApplicationCard.js`:

   ```jsx
   import React from 'react';
   import './ApplicationCard.css';

   function ApplicationCard({ application }) {
     const getStatusColor = (status) => {
       switch (status) {
         case 'Одобрена':
           return '#27ae60';
         case 'Отклонена':
           return '#e74c3c';
         case 'На рассмотрении':
           return '#f39c12';
         default:
           return '#7f8c8d';
       }
     };

     const getStatusIcon = (status) => {
       switch (status) {
         case 'Одобрена':
           return '✓';
         case 'Отклонена':
           return '✗';
         case 'На рассмотрении':
           return '⏳';
         default:
           return '?';
       }
     };

     return (
       <div className="application-card">
         <div className="application-header">
           <div className="competence-info">
             <h3>{application.competence}</h3>
             <div 
               className="status-badge"
               style={{ backgroundColor: getStatusColor(application.status) }}
             >
               <span className="status-icon">{getStatusIcon(application.status)}</span>
               {application.status}
             </div>
           </div>
           <div className="application-date">
             Дата подачи: {application.date}
           </div>
         </div>
         
         <div className="application-body">
           <p className="application-comment">
             <strong>Комментарий:</strong> {application.comment}
           </p>
           
           {application.documents && application.documents.length > 0 && (
             <div className="application-documents">
               <h4>Прикрепленные документы:</h4>
               <ul className="documents-list">
                 {application.documents.map((doc, index) => (
                   <li key={index} className="document-item">
                     <span className="document-name">📄 {doc}</span>
                     <button className="download-doc-btn">
                       Скачать
                     </button>
                   </li>
                 ))}
               </ul>
             </div>
           )}
         </div>
         
         <div className="application-actions">
           <button className="action-btn view-details">
             Подробнее
           </button>
           {application.status === 'На рассмотрении' && (
             <button className="action-btn cancel-application">
               Отозвать заявку
             </button>
           )}
         </div>
       </div>
     );
   }

   export default ApplicationCard;
   ```

3. **Создание стилей для ApplicationCard**
   В папке `src/components/` создайте файл `ApplicationCard.css`:

   ```css
   .application-card {
     background-color: white;
     border-radius: 10px;
     padding: 1.5rem;
     margin-bottom: 1.5rem;
     box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
     border-left: 4px solid #3498db;
     transition: transform 0.3s;
   }

   .application-card:hover {
     transform: translateY(-2px);
   }

   .application-header {
     display: flex;
     justify-content: space-between;
     align-items: flex-start;
     margin-bottom: 1.2rem;
     flex-wrap: wrap;
     gap: 1rem;
   }

   .competence-info h3 {
     margin: 0 0 0.5rem 0;
     color: #2c3e50;
     font-size: 1.3rem;
   }

   .status-badge {
     display: inline-flex;
     align-items: center;
     gap: 0.5rem;
     color: white;
     padding: 0.3rem 0.8rem;
     border-radius: 20px;
     font-size: 0.85rem;
     font-weight: 500;
   }

   .status-icon {
     font-weight: bold;
   }

   .application-date {
     color: #7f8c8d;
     font-size: 0.9rem;
     white-space: nowrap;
   }

   .application-body {
     margin-bottom: 1.5rem;
   }

   .application-comment {
     margin: 0 0 1.2rem 0;
     color: #34495e;
     line-height: 1.6;
   }

   .application-comment strong {
     color: #2c3e50;
   }

   .application-documents h4 {
     color: #2c3e50;
     margin-bottom: 0.8rem;
     font-size: 1rem;
   }

   .documents-list {
     list-style: none;
     padding: 0;
     margin: 0;
   }

   .document-item {
     display: flex;
     justify-content: space-between;
     align-items: center;
     padding: 0.8rem;
     background-color: #f8f9fa;
     border-radius: 5px;
     margin-bottom: 0.5rem;
   }

   .document-name {
     color: #2c3e50;
     font-size: 0.9rem;
   }

   .download-doc-btn {
     background-color: #3498db;
     color: white;
     border: none;
     padding: 0.4rem 1rem;
     border-radius: 3px;
     font-size: 0.85rem;
     cursor: pointer;
     transition: background-color 0.3s;
   }

   .download-doc-btn:hover {
     background-color: #2980b9;
   }

   .application-actions {
     display: flex;
     gap: 1rem;
   }

   .action-btn {
     padding: 0.6rem 1.5rem;
     border: none;
     border-radius: 5px;
     font-size: 0.9rem;
     cursor: pointer;
     transition: all 0.3s;
   }

   .view-details {
     background-color: #f8f9fa;
     color: #2c3e50;
   }

   .view-details:hover {
     background-color: #e9ecef;
   }

   .cancel-application {
     background-color: #e74c3c;
     color: white;
   }

   .cancel-application:hover {
     background-color: #c0392b;
   }

   @media (max-width: 768px) {
     .application-header {
       flex-direction: column;
     }
     
     .application-actions {
       flex-direction: column;
     }
     
     .document-item {
       flex-direction: column;
       align-items: flex-start;
       gap: 0.5rem;
     }
     
     .download-doc-btn {
       align-self: flex-end;
     }
   }
   ```

4. **Создание компонента NewApplicationForm**
   В папке `src/components/` создайте файл `NewApplicationForm.js`:

   ```jsx
   import React, { useState } from 'react';
   import './NewApplicationForm.css';

   function NewApplicationForm({ availableCompetencies, onSubmit }) {
     const [formData, setFormData] = useState({
       competence: '',
       motivation: '',
       experience: '',
       documents: []
     });

     const [fileInputs, setFileInputs] = useState([{ id: 1, file: null }]);

     const handleInputChange = (e) => {
       const { name, value } = e.target;
       setFormData(prev => ({
         ...prev,
         [name]: value
       }));
     };

     const handleFileChange = (id, file) => {
       const updatedInputs = fileInputs.map(input => 
         input.id === id ? { ...input, file } : input
       );
       setFileInputs(updatedInputs);
       
       // Собираем все файлы
       const files = updatedInputs
         .map(input => input.file)
         .filter(file => file !== null);
       
       setFormData(prev => ({
         ...prev,
         documents: files
       }));
     };

     const addFileInput = () => {
       const newId = fileInputs.length > 0 ? 
         Math.max(...fileInputs.map(input => input.id)) + 1 : 1;
       setFileInputs([...fileInputs, { id: newId, file: null }]);
     };

     const removeFileInput = (id) => {
       if (fileInputs.length > 1) {
         const updatedInputs = fileInputs.filter(input => input.id !== id);
         setFileInputs(updatedInputs);
         
         const files = updatedInputs
           .map(input => input.file)
           .filter(file => file !== null);
         
         setFormData(prev => ({
           ...prev,
           documents: files
         }));
       }
     };

     const handleSubmit = (e) => {
       e.preventDefault();
       
       if (!formData.competence) {
         alert('Выберите компетенцию');
         return;
       }
       
       if (onSubmit) {
         onSubmit(formData);
       }
       
       // Сброс формы
       setFormData({
         competence: '',
         motivation: '',
         experience: '',
         documents: []
       });
       setFileInputs([{ id: 1, file: null }]);
     };

     return (
       <div className="new-application-form">
         <h3>Новая заявка на участие</h3>
         
         <form onSubmit={handleSubmit}>
           <div className="form-group">
             <label htmlFor="competence">Выберите компетенцию *</label>
             <select
               id="competence"
               name="competence"
               value={formData.competence}
               onChange={handleInputChange}
               required
             >
               <option value="">-- Выберите компетенцию --</option>
               {availableCompetencies.map((competence, index) => (
                 <option key={index} value={competence}>
                   {competence}
                 </option>
               ))}
             </select>
           </div>
           
           <div className="form-group">
             <label htmlFor="motivation">Мотивационное письмо</label>
             <textarea
               id="motivation"
               name="motivation"
               value={formData.motivation}
               onChange={handleInputChange}
               rows="4"
               placeholder="Расскажите, почему вы хотите участвовать в этой компетенции..."
             />
           </div>
           
           <div className="form-group">
             <label htmlFor="experience">Опыт работы</label>
             <textarea
               id="experience"
               name="experience"
               value={formData.experience}
               onChange={handleInputChange}
               rows="3"
               placeholder="Опишите ваш опыт в выбранной области..."
             />
           </div>
           
           <div className="form-group">
             <label>Документы для заявки</label>
             {fileInputs.map((input) => (
               <div key={input.id} className="file-input-group">
                 <input
                   type="file"
                   onChange={(e) => handleFileChange(input.id, e.target.files[0])}
                   className="file-input"
                 />
                 <span className="file-name">
                   {input.file ? input.file.name : 'Файл не выбран'}
                 </span>
                 {fileInputs.length > 1 && (
                   <button
                     type="button"
                     className="remove-file-btn"
                     onClick={() => removeFileInput(input.id)}
                   >
                     ×
                   </button>
                 )}
               </div>
             ))}
             <button
               type="button"
               className="add-file-btn"
               onClick={addFileInput}
             >
               + Добавить еще файл
             </button>
             <p className="file-hint">Поддерживаемые форматы: PDF, DOC, DOCX, JPG, PNG. Максимальный размер: 10MB</p>
           </div>
           
           <div className="form-actions">
             <button type="submit" className="submit-btn">
               Подать заявку
             </button>
           </div>
         </form>
       </div>
     );
   }

   export default NewApplicationForm;
   ```

5. **Создание стилей для NewApplicationForm**
   В папке `src/components/` создайте файл `NewApplicationForm.css`:

   ```css
   .new-application-form {
     background-color: white;
     border-radius: 10px;
     padding: 2rem;
     box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
     margin-bottom: 2rem;
   }

   .new-application-form h3 {
     color: #2c3e50;
     margin-bottom: 1.5rem;
     padding-bottom: 1rem;
     border-bottom: 2px solid #f0f0f0;
   }

   .form-group {
     margin-bottom: 1.5rem;
   }

   .form-group label {
     display: block;
     margin-bottom: 0.5rem;
     font-weight: 500;
     color: #2c3e50;
   }

   .form-group select,
   .form-group textarea {
     width: 100%;
     padding: 0.8rem 1rem;
     border: 2px solid #ddd;
     border-radius: 5px;
     font-size: 1rem;
     font-family: inherit;
     transition: all 0.3s;
   }

   .form-group select:focus,
   .form-group textarea:focus {
     outline: none;
     border-color: #3498db;
     box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
   }

   .form-group textarea {
     resize: vertical;
     min-height: 100px;
   }

   .file-input-group {
     display: flex;
     align-items: center;
     gap: 1rem;
     margin-bottom: 0.8rem;
   }

   .file-input {
     flex: 1;
     padding: 0.5rem;
     border: 2px solid #ddd;
     border-radius: 5px;
   }

   .file-name {
     flex: 2;
     color: #7f8c8d;
     font-size: 0.9rem;
     overflow: hidden;
     text-overflow: ellipsis;
     white-space: nowrap;
   }

   .remove-file-btn {
     background-color: #e74c3c;
     color: white;
     border: none;
     width: 30px;
     height: 30px;
     border-radius: 50%;
     font-size: 1.2rem;
     cursor: pointer;
     display: flex;
     align-items: center;
     justify-content: center;
     transition: background-color 0.3s;
   }

   .remove-file-btn:hover {
     background-color: #c0392b;
   }

   .add-file-btn {
     background-color: transparent;
     color: #3498db;
     border: 2px dashed #3498db;
     padding: 0.6rem 1.5rem;
     border-radius: 5px;
     font-size: 0.9rem;
     cursor: pointer;
     transition: all 0.3s;
     margin-bottom: 0.5rem;
   }

   .add-file-btn:hover {
     background-color: #f0f7ff;
   }

   .file-hint {
     font-size: 0.85rem;
     color: #95a5a6;
     margin: 0.5rem 0 0;
   }

   .form-actions {
     margin-top: 2rem;
   }

   .submit-btn {
     background-color: #27ae60;
     color: white;
     border: none;
     padding: 1rem 2rem;
     border-radius: 5px;
     font-size: 1.1rem;
     font-weight: 500;
     cursor: pointer;
     transition: background-color 0.3s;
     width: 100%;
   }

   .submit-btn:hover {
     background-color: #219653;
   }

   @media (max-width: 768px) {
     .new-application-form {
       padding: 1.5rem;
     }
     
     .file-input-group {
       flex-direction: column;
       align-items: flex-start;
       gap: 0.5rem;
     }
     
     .file-input,
     .file-name {
       width: 100%;
     }
   }
   ```

6. **Создание страницы PersonalCabinet**
   Обновите `src/pages/PersonalCabinet.js`:

   ```jsx
   import React, { useState } from 'react';
   import { userData, userApplications, availableCompetencies } from '../data/userData';
   import ApplicationCard from '../components/ApplicationCard';
   import NewApplicationForm from '../components/NewApplicationForm';
   import './PersonalCabinet.css';

   function PersonalCabinet() {
     const [applications, setApplications] = useState(userApplications);
     const [showNewForm, setShowNewForm] = useState(false);

     const handleNewApplication = (formData) => {
       const newApplication = {
         id: applications.length + 1,
         competence: formData.competence,
         status: 'На рассмотрении',
         date: new Date().toISOString().split('T')[0],
         comment: 'Заявка только что подана',
         documents: formData.documents.map(file => file.name)
       };
       
       setApplications([newApplication, ...applications]);
       setShowNewForm(false);
       alert('Заявка успешно подана!');
     };

     const handleCancelApplication = (applicationId) => {
       if (window.confirm('Вы уверены, что хотите отозвать заявку?')) {
         setApplications(applications.filter(app => app.id !== applicationId));
         alert('Заявка отозвана');
       }
     };

     const stats = {
       total: applications.length,
       approved: applications.filter(app => app.status === 'Одобрена').length,
       pending: applications.filter(app => app.status === 'На рассмотрении').length,
       rejected: applications.filter(app => app.status === 'Отклонена').length
     };

     return (
       <div className="personal-cabinet">
         {/* Заголовок и информация о пользователе */}
         <div className="user-profile-header">
           <div className="user-avatar">
             <img src={userData.photo} alt={userData.fullName} />
           </div>
           <div className="user-info">
             <h1>Личный кабинет</h1>
             <h2>{userData.fullName}</h2>
             <div className="user-details">
               <div className="detail">
                 <span className="label">Email:</span>
                 <span className="value">{userData.email}</span>
               </div>
               <div className="detail">
                 <span className="label">Категория:</span>
                 <span className="value">{userData.category}</span>
               </div>
               <div className="detail">
                 <span className="label">Регион:</span>
                 <span className="value">{userData.region}</span>
               </div>
               <div className="detail">
                 <span className="label">Учебное заведение:</span>
                 <span className="value">{userData.university}</span>
               </div>
             </div>
           </div>
         </div>
         
         {/* Статистика заявок */}
         <div className="applications-stats">
           <div className="stat-card">
             <div className="stat-number">{stats.total}</div>
             <div className="stat-label">Всего заявок</div>
           </div>
           <div className="stat-card approved">
             <div className="stat-number">{stats.approved}</div>
             <div className="stat-label">Одобрено</div>
           </div>
           <div className="stat-card pending">
             <div className="stat-number">{stats.pending}</div>
             <div className="stat-label">На рассмотрении</div>
           </div>
           <div className="stat-card rejected">
             <div className="stat-number">{stats.rejected}</div>
             <div className="stat-label">Отклонено</div>
           </div>
         </div>
         
         {/* Кнопка новой заявки */}
         <div className="new-application-section">
           {!showNewForm ? (
             <button 
               className="new-application-btn"
               onClick={() => setShowNewForm(true)}
             >
               + Подать новую заявку
             </button>
           ) : (
             <div className="new-application-container">
               <NewApplicationForm 
                 availableCompetencies={availableCompetencies}
                 onSubmit={handleNewApplication}
               />
               <button 
                 className="cancel-btn"
                 onClick={() => setShowNewForm(false)}
               >
                 Отмена
               </button>
             </div>
           )}
         </div>
         
         {/* Список заявок */}
         <div className="applications-list">
           <h3>Мои заявки</h3>
           
           {applications.length > 0 ? (
             <div className="applications-container">
               {applications.map((application) => (
                 <ApplicationCard 
                   key={application.id} 
                   application={application}
                 />
               ))}
             </div>
           ) : (
             <div className="no-applications">
               <p>У вас пока нет заявок</p>
               <button 
                 className="create-first-btn"
                 onClick={() => setShowNewForm(true)}
               >
                 Создать первую заявку
               </button>
             </div>
           )}
         </div>
       </div>
     );
   }

   export default PersonalCabinet;
   ```

7. **Создание стилей для PersonalCabinet**
   В папке `src/pages/` создайте файл `PersonalCabinet.css`:

   ```css
   .personal-cabinet {
     max-width: 1000px;
     margin: 0 auto;
   }

   /* Шапка профиля */
   .user-profile-header {
     display: flex;
     align-items: flex-start;
     gap: 2rem;
     background-color: white;
     border-radius: 10px;
     padding: 2rem;
     box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
     margin-bottom: 2rem;
   }

   .user-avatar {
     flex-shrink: 0;
   }

   .user-avatar img {
     width: 150px;
     height: 150px;
     border-radius: 50%;
     object-fit: cover;
     border: 5px solid #f8f9fa;
     box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
   }

   .user-info {
     flex: 1;
   }

   .user-info h1 {
     color: #2c3e50;
     margin-bottom: 0.5rem;
     font-size: 1.8rem;
   }

   .user-info h2 {
     color: #3498db;
     margin-bottom: 1.5rem;
     font-size: 1.5rem;
   }

   .user-details {
     display: grid;
     grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
     gap: 1rem;
   }

   .detail {
     display: flex;
     flex-direction: column;
   }

   .detail .label {
     font-weight: 500;
     color: #7f8c8d;
     font-size: 0.9rem;
     margin-bottom: 0.2rem;
   }

   .detail .value {
     color: #2c3e50;
     font-size: 1rem;
   }

   /* Статистика */
   .applications-stats {
     display: grid;
     grid-template-columns: repeat(4, 1fr);
     gap: 1rem;
     margin-bottom: 2rem;
   }

   .stat-card {
     background-color: white;
     border-radius: 8px;
     padding: 1.5rem;
     text-align: center;
     box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
     border-top: 4px solid #3498db;
   }

   .stat-card.approved {
     border-top-color: #27ae60;
   }

   .stat-card.pending {
     border-top-color: #f39c12;
   }

   .stat-card.rejected {
     border-top-color: #e74c3c;
   }

   .stat-number {
     font-size: 2.5rem;
     font-weight: bold;
     color: #2c3e50;
     margin-bottom: 0.5rem;
   }

   .stat-label {
     color: #7f8c8d;
     font-size: 0.9rem;
     text-transform: uppercase;
     letter-spacing: 1px;
   }

   /* Новая заявка */
   .new-application-section {
     margin-bottom: 2rem;
   }

   .new-application-btn {
     background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
     color: white;
     border: none;
     padding: 1.2rem 2.5rem;
     border-radius: 8px;
     font-size: 1.1rem;
     font-weight: 500;
     cursor: pointer;
     transition: transform 0.3s, box-shadow 0.3s;
     width: 100%;
   }

   .new-application-btn:hover {
     transform: translateY(-2px);
     box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
   }

   .new-application-container {
     position: relative;
   }

   .cancel-btn {
     background-color: #95a5a6;
     color: white;
     border: none;
     padding: 0.8rem 1.5rem;
     border-radius: 5px;
     font-size: 1rem;
     cursor: pointer;
     transition: background-color 0.3s;
     margin-top: 1rem;
     width: 100%;
   }

   .cancel-btn:hover {
     background-color: #7f8c8d;
   }

   /* Список заявок */
   .applications-list h3 {
     color: #2c3e50;
     margin-bottom: 1.5rem;
     padding-bottom: 0.8rem;
     border-bottom: 2px solid #f0f0f0;
   }

   .no-applications {
     text-align: center;
     padding: 4rem 2rem;
     background-color: #f8f9fa;
     border-radius: 10px;
     border: 2px dashed #ddd;
   }

   .no-applications p {
     font-size: 1.2rem;
     color: #7f8c8d;
     margin-bottom: 1.5rem;
   }

   .create-first-btn {
     background-color: #3498db;
     color: white;
     border: none;
     padding: 1rem 2rem;
     border-radius: 5px;
     font-size: 1rem;
     cursor: pointer;
     transition: background-color 0.3s;
   }

   .create-first-btn:hover {
     background-color: #2980b9;
   }

   /* Адаптивность */
   @media (max-width: 768px) {
     .user-profile-header {
       flex-direction: column;
       align-items: center;
       text-align: center;
       padding: 1.5rem;
     }
     
     .user-avatar img {
       width: 120px;
       height: 120px;
     }
     
     .user-details {
       grid-template-columns: 1fr;
     }
     
     .applications-stats {
       grid-template-columns: repeat(2, 1fr);
     }
     
     .stat-number {
       font-size: 2rem;
     }
   }

   @media (max-width: 480px) {
     .applications-stats {
       grid-template-columns: 1fr;
     }
     
     .user-profile-header {
       padding: 1rem;
     }
     
     .user-info h1 {
       font-size: 1.5rem;
     }
     
     .user-info h2 {
       font-size: 1.2rem;
     }
   }
   ```

8. **Создание простой страницы Login**
   Обновите `src/pages/Login.js`:

   ```jsx
   import React, { useState } from 'react';
   import { Link } from 'react-router-dom';
   import './Login.css';

   function Login() {
     const [formData, setFormData] = useState({
       email: '',
       password: '',
       rememberMe: false
     });

     const handleSubmit = (e) => {
       e.preventDefault();
       // В реальном приложении здесь будет запрос к API
       console.log('Вход:', formData);
       alert('Вход выполнен (заглушка)');
     };

     return (
       <div className="login-page">
         <div className="login-container">
           <div className="login-header">
             <h1>Вход в личный кабинет</h1>
             <p>Введите ваши учетные данные для доступа к личному кабинету</p>
           </div>
           
           <form onSubmit={handleSubmit} className="login-form">
             <div className="form-group">
               <label htmlFor="email">Email</label>
               <input
                 id="email"
                 type="email"
                 value={formData.email}
                 onChange={(e) => setFormData({...formData, email: e.target.value})}
                 placeholder="example@mail.ru"
                 required
               />
             </div>
             
             <div className="form-group">
               <label htmlFor="password">Пароль</label>
               <input
                 id="password"
                 type="password"
                 value={formData.password}
                 onChange={(e) => setFormData({...formData, password: e.target.value})}
                 placeholder="Введите пароль"
                 required
               />
             </div>
             
             <div className="form-options">
               <label className="checkbox-label">
                 <input
                   type="checkbox"
                   checked={formData.rememberMe}
                   onChange={(e) => setFormData({...formData, rememberMe: e.target.checked})}
                 />
                 <span>Запомнить меня</span>
               </label>
               
               <Link to="/reset-password" className="forgot-password">
                 Забыли пароль?
               </Link>
             </div>
             
             <button type="submit" className="login-btn">
               Войти
             </button>
           </form>
           
           <div className="login-footer">
             <p>Еще нет аккаунта?</p>
             <Link to="/registration" className="register-link">
               Зарегистрироваться
             </Link>
           </div>
           
           <div className="login-info">
             <div className="info-card">
               <h3>Участникам</h3>
               <p>Войдите для подачи заявок на участие и отслеживания их статуса</p>
             </div>
             <div className="info-card">
               <h3>Экспертам</h3>
               <p>Доступ к системе оценки участников и управлению компетенциями</p>
             </div>
           </div>
         </div>
       </div>
     );
   }

   export default Login;
   ```

9. **Создание стилей для Login**
   В папке `src/pages/` создайте файл `Login.css`:

   ```css
   .login-page {
     max-width: 500px;
     margin: 0 auto;
     padding: 2rem 1rem;
   }

   .login-container {
     background-color: white;
     border-radius: 10px;
     padding: 2.5rem;
     box-shadow: 0 5px 20px rgba(0, 0, 0, 0.1);
   }

   .login-header {
     text-align: center;
     margin-bottom: 2rem;
   }

   .login-header h1 {
     color: #2c3e50;
     margin-bottom: 0.8rem;
   }

   .login-header p {
     color: #7f8c8d;
     margin: 0;
   }

   .login-form {
     margin-bottom: 2rem;
   }

   .form-group {
     margin-bottom: 1.5rem;
   }

   .form-group label {
     display: block;
     margin-bottom: 0.5rem;
     font-weight: 500;
     color: #2c3e50;
   }

   .form-group input {
     width: 100%;
     padding: 0.8rem 1rem;
     border: 2px solid #ddd;
     border-radius: 5px;
     font-size: 1rem;
     transition: all 0.3s;
   }

   .form-group input:focus {
     outline: none;
     border-color: #3498db;
     box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
   }

   .form-options {
     display: flex;
     justify-content: space-between;
     align-items: center;
     margin-bottom: 1.5rem;
   }

   .checkbox-label {
     display: flex;
     align-items: center;
     gap: 0.5rem;
     cursor: pointer;
     color: #2c3e50;
   }

   .checkbox-label input {
     width: auto;
   }

   .forgot-password {
     color: #3498db;
     text-decoration: none;
     font-size: 0.9rem;
   }

   .forgot-password:hover {
     text-decoration: underline;
   }

   .login-btn {
     background-color: #3498db;
     color: white;
     border: none;
     padding: 1rem;
     border-radius: 5px;
     font-size: 1.1rem;
     font-weight: 500;
     cursor: pointer;
     transition: background-color 0.3s;
     width: 100%;
   }

   .login-btn:hover {
     background-color: #2980b9;
   }

   .login-footer {
     text-align: center;
     padding-top: 1.5rem;
     border-top: 1px solid #eee;
     margin-bottom: 2rem;
   }

   .login-footer p {
     color: #7f8c8d;
     margin-bottom: 0.5rem;
   }

   .register-link {
     color: #3498db;
     text-decoration: none;
     font-weight: 500;
     font-size: 1.1rem;
   }

   .register-link:hover {
     text-decoration: underline;
   }

   .login-info {
     display: grid;
     grid-template-columns: repeat(2, 1fr);
     gap: 1.5rem;
     margin-top: 2rem;
   }

   .info-card {
     background-color: #f8f9fa;
     padding: 1.2rem;
     border-radius: 8px;
     border-left: 4px solid #3498db;
   }

   .info-card h3 {
     color: #2c3e50;
     margin-bottom: 0.5rem;
     font-size: 1rem;
   }

   .info-card p {
     color: #7f8c8d;
     margin: 0;
     font-size: 0.9rem;
     line-height: 1.5;
   }

   @media (max-width: 768px) {
     .login-container {
       padding: 2rem;
     }
     
     .login-info {
       grid-template-columns: 1fr;
     }
   }

   @media (max-width: 480px) {
     .login-container {
       padding: 1.5rem;
     }
     
     .form-options {
       flex-direction: column;
       align-items: flex-start;
       gap: 0.8rem;
     }
     
     .login-header h1 {
       font-size: 1.5rem;
     }
   }
   ```

10. **Создание простой страницы AdminPanel (базовый вариант)**
    Обновите `src/pages/AdminPanel.js`:

    ```jsx
    import React, { useState } from 'react';
    import './AdminPanel.css';

    function AdminPanel() {
      const [competences, setCompetences] = useState([
        { id: 1, name: 'Веб-разработка', description: 'Разработка веб-приложений', taskFile: null },
        { id: 2, name: 'Графический дизайн', description: 'Создание визуального контента', taskFile: null }
      ]);

      const [newCompetence, setNewCompetence] = useState({
        name: '',
        description: '',
        taskFile: null
      });

      const handleSubmit = (e) => {
        e.preventDefault();
        
        if (!newCompetence.name.trim()) {
          alert('Введите название компетенции');
          return;
        }

        const newComp = {
          id: competences.length + 1,
          ...newCompetence
        };

        setCompetences([...competences, newComp]);
        setNewCompetence({ name: '', description: '', taskFile: null });
        alert('Компетенция добавлена');
      };

      const handleFileChange = (e) => {
        const file = e.target.files[0];
        if (file) {
          setNewCompetence(prev => ({
            ...prev,
            taskFile: file
          }));
        }
      };

      return (
        <div className="admin-panel">
          <div className="admin-header">
            <h1>Панель администратора</h1>
            <p>Управление компетенциями и настройки системы</p>
          </div>
          
          <div className="admin-content">
            <div className="admin-section">
              <h2>Добавить новую компетенцию</h2>
              
              <form onSubmit={handleSubmit} className="competence-form">
                <div className="form-group">
                  <label htmlFor="name">Название компетенции *</label>
                  <input
                    id="name"
                    type="text"
                    value={newCompetence.name}
                    onChange={(e) => setNewCompetence({...newCompetence, name: e.target.value})}
                    placeholder="Веб-разработка"
                    required
                  />
                </div>
                
                <div className="form-group">
                  <label htmlFor="description">Описание</label>
                  <textarea
                    id="description"
                    value={newCompetence.description}
                    onChange={(e) => setNewCompetence({...newCompetence, description: e.target.value})}
                    placeholder="Краткое описание компетенции..."
                    rows="3"
                  />
                </div>
                
                <div className="form-group">
                  <label htmlFor="taskFile">Файл с заданием</label>
                  <input
                    id="taskFile"
                    type="file"
                    onChange={handleFileChange}
                    accept=".pdf,.doc,.docx"
                  />
                  <p className="file-hint">PDF, DOC, DOCX. Максимальный размер: 10MB</p>
                </div>
                
                <button type="submit" className="submit-btn">
                  Добавить компетенцию
                </button>
              </form>
            </div>
            
            <div className="admin-section">
              <h2>Существующие компетенции</h2>
              
              <div className="competences-list">
                {competences.length > 0 ? (
                  <div className="competences-grid">
                    {competences.map((comp) => (
                      <div key={comp.id} className="competence-card">
                        <h3>{comp.name}</h3>
                        <p>{comp.description}</p>
                        <div className="competence-actions">
                          <button className="edit-btn">Редактировать</button>
                          <button className="delete-btn">Удалить</button>
                        </div>
                      </div>
                    ))}
                  </div>
                ) : (
                  <p className="no-data">Нет добавленных компетенций</p>
                )}
              </div>
            </div>
            
            <div className="admin-stats">
              <div className="stat-card">
                <div className="stat-number">{competences.length}</div>
                <div className="stat-label">Компетенций</div>
              </div>
              <div className="stat-card">
                <div className="stat-number">0</div>
                <div className="stat-label">Заявок сегодня</div>
              </div>
              <div className="stat-card">
                <div className="stat-number">0</div>
                <div className="stat-label">Новых пользователей</div>
              </div>
            </div>
          </div>
        </div>
      );
    }

    export default AdminPanel;
    ```

11. **Создание стилей для AdminPanel**
    В папке `src/pages/` создайте файл `AdminPanel.css`:

    ```css
    .admin-panel {
      max-width: 1200px;
      margin: 0 auto;
    }

    .admin-header {
      text-align: center;
      margin-bottom: 3rem;
    }

    .admin-header h1 {
      color: #2c3e50;
      margin-bottom: 0.5rem;
    }

    .admin-header p {
      color: #7f8c8d;
      font-size: 1.1rem;
    }

    .admin-content {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 2rem;
    }

    .admin-section {
      background-color: white;
      border-radius: 10px;
      padding: 2rem;
      box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
    }

    .admin-section h2 {
      color: #2c3e50;
      margin-bottom: 1.5rem;
      padding-bottom: 1rem;
      border-bottom: 2px solid #f0f0f0;
    }

    .competence-form {
      display: flex;
      flex-direction: column;
      gap: 1.5rem;
    }

    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
      color: #2c3e50;
    }

    .form-group input,
    .form-group textarea {
      width: 100%;
      padding: 0.8rem 1rem;
      border: 2px solid #ddd;
      border-radius: 5px;
      font-size: 1rem;
      font-family: inherit;
    }

    .form-group input:focus,
    .form-group textarea:focus {
      outline: none;
      border-color: #3498db;
      box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
    }

    .file-hint {
      font-size: 0.85rem;
      color: #95a5a6;
      margin: 0.5rem 0 0;
    }

    .submit-btn {
      background-color: #27ae60;
      color: white;
      border: none;
      padding: 1rem 2rem;
      border-radius: 5px;
      font-size: 1.1rem;
      font-weight: 500;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .submit-btn:hover {
      background-color: #219653;
    }

    .competences-grid {
      display: grid;
      gap: 1rem;
    }

    .competence-card {
      background-color: #f8f9fa;
      border-radius: 8px;
      padding: 1.2rem;
      border-left: 4px solid #3498db;
    }

    .competence-card h3 {
      margin: 0 0 0.5rem 0;
      color: #2c3e50;
    }

    .competence-card p {
      margin: 0 0 1rem 0;
      color: #7f8c8d;
      font-size: 0.9rem;
    }

    .competence-actions {
      display: flex;
      gap: 0.5rem;
    }

    .edit-btn {
      background-color: #3498db;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 3px;
      font-size: 0.85rem;
      cursor: pointer;
    }

    .delete-btn {
      background-color: #e74c3c;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 3px;
      font-size: 0.85rem;
      cursor: pointer;
    }

    .no-data {
      text-align: center;
      color: #95a5a6;
      font-style: italic;
      padding: 2rem;
      background-color: #f8f9fa;
      border-radius: 8px;
    }

    .admin-stats {
      grid-column: span 2;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 1rem;
    }

    .admin-stats .stat-card {
      background-color: white;
      border-radius: 8px;
      padding: 1.5rem;
      text-align: center;
      box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
      border-top: 4px solid #3498db;
    }

    .admin-stats .stat-number {
      font-size: 2rem;
      font-weight: bold;
      color: #2c3e50;
      margin-bottom: 0.5rem;
    }

    .admin-stats .stat-label {
      color: #7f8c8d;
      font-size: 0.9rem;
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    @media (max-width: 992px) {
      .admin-content {
        grid-template-columns: 1fr;
      }
      
      .admin-stats {
        grid-column: span 1;
        grid-template-columns: repeat(3, 1fr);
      }
    }

    @media (max-width: 768px) {
      .admin-section {
        padding: 1.5rem;
      }
      
      .admin-stats {
        grid-template-columns: 1fr;
      }
      
      .competence-actions {
        flex-direction: column;
      }
    }
    ```

**Проверка:**
1. Перейдите на страницу `/personal-cabinet`
2. Проверьте отображение информации о пользователе
3. Проверьте статистику заявок
4. Нажмите "Подать новую заявку"
5. Проверьте форму подачи заявки
6. Проверьте отображение существующих заявок
7. Проверьте страницу `/login`
8. Проверьте страницу `/admin`
9. Проверьте адаптивность всех страниц

---

На этом основные страницы фронтенда готовы! В следующей части мы настроим глобальное состояние, обработку ошибок и финальные доработки.

*Продолжить?*
