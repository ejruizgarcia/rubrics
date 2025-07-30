import React, { useState, useEffect, useRef } from 'react';
// Firebase imports - These are handled by the environment.
import { initializeApp } from 'firebase/app';
import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from 'firebase/auth';
import { getFirestore, doc, getDoc, setDoc, onSnapshot, collection, addDoc, deleteDoc, query, updateDoc, serverTimestamp, orderBy } from 'firebase/firestore';

// --- Helper Components ---

const Modal = ({ children, onClose, size = 'max-w-4xl', customClasses = '' }) => {
    const overlayRef = useRef();

    const handleOverlayClick = (e) => {
        if (e.target === overlayRef.current) {
            onClose();
        }
    };

    return (
        <div ref={overlayRef} onClick={handleOverlayClick} className={`fixed inset-0 bg-black bg-opacity-60 z-40 flex justify-center items-center p-4 ${customClasses}`}>
            <div className={`bg-white rounded-2xl shadow-2xl w-full ${size} max-h-[90vh] flex flex-col relative`}>
                {children}
            </div>
        </div>
    );
};

const ConfirmationModal = ({ onConfirm, onCancel, title, message, confirmText = "Confirmar Borrado", cancelText = "Cancelar" }) => {
    return (
        <Modal onClose={onCancel} size="max-w-md" customClasses="z-50">
            <div className="p-6">
                <h2 className="text-xl font-bold mb-4">{title}</h2>
                <p>{message}</p>
            </div>
            <div className="p-4 bg-gray-50 border-t flex justify-end space-x-3">
                <button onClick={onCancel} className="bg-gray-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-600">{cancelText}</button>
                <button onClick={onConfirm} className="bg-red-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-red-700">{confirmText}</button>
            </div>
        </Modal>
    );
};

const DependencyModal = ({ name, activities, onClose }) => {
    return (
        <Modal onClose={onClose} size="max-w-lg">
            <div className="p-6">
                <h2 className="text-xl font-bold mb-4">No se puede eliminar "{name}"</h2>
                <p className="text-gray-700 mb-4">
                    Este elemento no se puede eliminar porque existe/n actividad/es vinculada/s:
                </p>
                <ul className="list-disc list-inside bg-gray-100 p-3 rounded-lg max-h-48 overflow-y-auto">
                    {activities.map(activity => (
                        <li key={activity.id} className="text-gray-800">{activity.title}</li>
                    ))}
                </ul>
            </div>
            <div className="p-4 bg-gray-50 border-t flex justify-end">
                <button onClick={onClose} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Entendido</button>
            </div>
        </Modal>
    );
};

const InfoModal = ({ title, message, link, onClose, customClasses = '' }) => (
    <Modal onClose={onClose} size="max-w-md" customClasses={customClasses}>
        <div className="p-6">
            <h2 className="text-xl font-bold mb-4">{title}</h2>
            <p className="text-gray-700">{message}</p>
            {link && <a href={link} target="_blank" rel="noopener noreferrer" className="text-blue-600 hover:underline font-semibold mt-4 inline-block">Ver plantilla de ejemplo</a>}
        </div>
        <div className="p-4 bg-gray-50 border-t flex justify-end">
            <button onClick={onClose} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Entendido</button>
        </div>
    </Modal>
);


const Message = ({ text, type }) => {
    if (!text) return null;
    const baseClasses = "p-4 mb-4 text-sm rounded-lg fixed top-5 right-5 z-50 shadow-lg";
    const typeClasses = {
        success: "bg-green-100 text-green-800",
        error: "bg-red-100 text-red-800",
        loading: "bg-blue-100 text-blue-800",
    };
    return (
        <div className={`${baseClasses} ${typeClasses[type] || typeClasses.success}`} role="alert">
            <span className="font-medium">{text}</span>
        </div>
    );
};

const Spinner = () => <div className="animate-spin rounded-full h-8 w-8 border-b-2 border-gray-900"></div>;
const ButtonSpinner = () => <div className="animate-spin rounded-full h-5 w-5 border-b-2 border-white"></div>;


// --- Main App Component ---

const App = () => {
    // --- State Variables ---
    const [db, setDb] = useState(null);
    const [auth, setAuth] = useState(null);
    const [userId, setUserId] = useState(null);
    const [isAuthReady, setIsAuthReady] = useState(false);
    const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-rubric-app-v40';

    const [studentsInput, setStudentsInput] = useState('');
    const [classNameInput, setClassNameInput] = useState('');
    const [currentClass, setCurrentClass] = useState(null);
    const [savedClasses, setSavedClasses] = useState([]);

    const [rubricFileInput, setRubricFileInput] = useState(null);
    const [rubricNameInput, setRubricNameInput] = useState('');
    const [currentRubric, setCurrentRubric] = useState(null);
    const [savedRubrics, setSavedRubrics] = useState([]);

    const [selectedStudent, setSelectedStudent] = useState(null);
    const [evaluations, setEvaluations] = useState({});
    const [tempEvaluation, setTempEvaluation] = useState(null);
    
    const [message, setMessage] = useState({ text: '', type: '' });
    const [isJsPDFLoaded, setIsJsPDFLoaded] = useState(false);
    const [searchTerm, setSearchTerm] = useState('');
    const [currentPage, setCurrentPage] = useState('main'); // 'main', 'evaluation', 'reportPreview'
    
    const [llmFeedback, setLlmFeedback] = useState('');
    const [isGeneratingFeedback, setIsGeneratingFeedback] = useState(false);
    const [isSuggestingCriteria, setIsSuggestingCriteria] = useState(false);
    const [isGeneratingRubric, setIsGeneratingRubric] = useState(false);

    // Activity State
    const [savedActivities, setSavedActivities] = useState([]);
    const [currentActivityId, setCurrentActivityId] = useState(null);
    const [activitySearchTerm, setActivitySearchTerm] = useState('');
    const [selectedClassFilter, setSelectedClassFilter] = useState('');
    const [selectedRubricFilter, setSelectedRubricFilter] = useState('');

    // Evaluation Criteria State
    const [savedEvalCriteria, setSavedEvalCriteria] = useState([]);
    const [showCreateEvalCriteriaModal, setShowCreateEvalCriteriaModal] = useState(false);
    const [criteriaToEdit, setCriteriaToEdit] = useState(null);
    const [newCriteriaSubject, setNewCriteriaSubject] = useState('');
    const [newCriteriaCourse, setNewCriteriaCourse] = useState('');
    const [tableCriteria, setTableCriteria] = useState([{ id: Date.now(), code: '', text: '' }]);
    const [criteriaSetToAssign, setCriteriaSetToAssign] = useState('');
    const [selectedCriteriaForActivity, setSelectedCriteriaForActivity] = useState([]);
    const [showImportCriteriaModal, setShowImportCriteriaModal] = useState(false);
    const [criteriaMdFile, setCriteriaMdFile] = useState(null);


    // Modals and Report State
    const [showEvalModal, setShowEvalModal] = useState(false);
    const [showCreateClassModal, setShowCreateClassModal] = useState(false);
    const [showEditClassModal, setShowEditClassModal] = useState(false);
    const [classToEdit, setClassToEdit] = useState(null);
    const [showEditRubricModal, setShowEditRubricModal] = useState(false);
    const [rubricToEdit, setRubricToEdit] = useState(null);
    const [editableRubricData, setEditableRubricData] = useState(null);
    const [newStudentName, setNewStudentName] = useState("");
    const [reportPreviewData, setReportPreviewData] = useState(null);
    const [showDeleteConfirmModal, setShowDeleteConfirmModal] = useState(false);
    const [itemToDelete, setItemToDelete] = useState(null);
    const [showActivityTitleModal, setShowActivityTitleModal] = useState(false);
    const [activityTitle, setActivityTitle] = useState("");
    const [activityTitleInput, setActivityTitleInput] = useState("");
    const [showInfoModal, setShowInfoModal] = useState(false);
    const [showDuplicateRubricModal, setShowDuplicateRubricModal] = useState(false);
    const [pendingRubric, setPendingRubric] = useState(null);
    const [newRubricName, setNewRubricName] = useState("");
    const [showDependencyModal, setShowDependencyModal] = useState(false);
    const [dependencyInfo, setDependencyInfo] = useState({ name: '', activities: [] });
    const [showValidationErrorModal, setShowValidationErrorModal] = useState(false);
    const [validationErrorInfo, setValidationErrorInfo] = useState({ title: '', message: ''});
    const [showCloseEvalConfirmModal, setShowCloseEvalConfirmModal] = useState(false);
    const [showGenerateRubricModal, setShowGenerateRubricModal] = useState(false);
    const [generateRubricTitle, setGenerateRubricTitle] = useState('');
    const [generateRubricDesc, setGenerateRubricDesc] = useState('');
    const [generateRubricCriteriaSet, setGenerateRubricCriteriaSet] = useState('');
    const [generateRubricSelectedCriteria, setGenerateRubricSelectedCriteria] = useState([]);
    const [showManageEvalCriteriaModal, setShowManageEvalCriteriaModal] = useState(false);

    // --- Utility Functions ---
    const showMessage = (text, type = 'success', duration = 3000) => {
        setMessage({ text, type });
        if (duration) {
            setTimeout(() => setMessage({ text: '', type: '' }), duration);
        }
    };
    
    const formatDate = (timestamp) => {
        if (!timestamp?.toDate) return '';
        const date = timestamp.toDate();
        const month = (date.getMonth() + 1).toString().padStart(2, '0');
        const day = date.getDate().toString().padStart(2, '0');
        const year = date.getFullYear();
        return `${month}/${day}/${year}`;
    };

    const parseCriterionNameAndWeight = (nameString) => {
        if (!nameString) return { name: '', weight: null };
        const cleanedString = nameString.replace(/\*\*/g, '');
        const match = cleanedString.match(/(.*)\s*\((\d{1,3})%\s*\)/);
        if (match) {
            return { name: match[1].trim(), weight: parseInt(match[2], 10) };
        }
        return { name: cleanedString.trim(), weight: null };
    };

    // --- Firebase & Data Initialization ---
    useEffect(() => {
        try {
            const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
            const app = initializeApp(firebaseConfig);
            const firestoreDb = getFirestore(app);
            const firebaseAuth = getAuth(app);
            setDb(firestoreDb);
            setAuth(firebaseAuth);

            onAuthStateChanged(firebaseAuth, async (user) => {
                if (user) {
                    setUserId(user.uid);
                } else {
                    try {
                        const token = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                        if (token) await signInWithCustomToken(firebaseAuth, token);
                        else await signInAnonymously(firebaseAuth);
                    } catch (authError) {
                        console.error("Firebase Auth Error:", authError);
                        showMessage("Error de autenticación.", "error", 0);
                    }
                }
                setIsAuthReady(true);
            });
        } catch (e) {
            console.error("Error initializing Firebase:", e);
            showMessage("Error de configuración de Firebase.", "error", 0);
        }
    }, []);

    useEffect(() => {
        if (!isAuthReady || !db || !userId) return;
        const commonPath = `artifacts/${appId}/users/${userId}`;

        const unsubClasses = onSnapshot(query(collection(db, `${commonPath}/classes`)), (snap) => {
            setSavedClasses(snap.docs.map(d => ({ id: d.id, ...d.data() })));
        });

        const unsubRubrics = onSnapshot(query(collection(db, `${commonPath}/rubrics`)), (snap) => {
            setSavedRubrics(snap.docs.map(d => ({ id: d.id, ...d.data() })));
        });

        const unsubActivities = onSnapshot(query(collection(db, `${commonPath}/activities`), orderBy("createdAt", "desc")), (snap) => {
            setSavedActivities(snap.docs.map(d => ({ id: d.id, ...d.data() })));
        });

        const unsubEvalCriteria = onSnapshot(query(collection(db, `${commonPath}/evalCriteria`)), (snap) => {
            setSavedEvalCriteria(snap.docs.map(d => ({ id: d.id, ...d.data() })));
        });

        return () => { unsubClasses(); unsubRubrics(); unsubActivities(); unsubEvalCriteria(); };
    }, [isAuthReady, db, userId, appId]);
    
    // Listener for evaluations of the current activity
    useEffect(() => {
        if (!db || !userId || !currentActivityId) {
            setEvaluations({});
            return;
        };
        
        const unsubEvals = onSnapshot(doc(db, `artifacts/${appId}/users/${userId}/evaluations`, currentActivityId), (snap) => {
            setEvaluations(snap.exists() ? snap.data() : {});
        });

        return () => unsubEvals();
    }, [db, userId, currentActivityId]);

    // --- Library Loading ---
    useEffect(() => {
        const loadScript = (src, onload) => {
            if (document.querySelector(`script[src="${src}"]`)) {
                if (window.jspdf?.jsPDF?.autoTable) setIsJsPDFLoaded(true);
                return;
            }
            const script = document.createElement('script');
            script.src = src;
            script.onload = onload;
            document.body.appendChild(script);
        };
        if (!window.jspdf) {
            loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js', () => {
                loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.23/jspdf.plugin.autotable.min.js', () => {
                    setIsJsPDFLoaded(true);
                });
            });
        } else {
             setIsJsPDFLoaded(true);
        }
    }, []);

    // --- Class Management ---
    const handleSaveClass = async () => {
        if (!classNameInput.trim()) return showMessage("Introduce un nombre para la clase.", "error");
        const studentNames = studentsInput.split('\n').map(name => name.trim()).filter(Boolean);
        if (studentNames.length === 0) return showMessage("Añade al menos un estudiante.", "error");
        
        const newClassData = {
            name: classNameInput.trim(),
            students: studentNames.map((name, i) => ({ id: `student-${Date.now()}-${i}`, name }))
        };

        try {
            const existing = savedClasses.find(c => c.name === newClassData.name);
            if (existing) {
                await setDoc(doc(db, `artifacts/${appId}/users/${userId}/classes`, existing.id), newClassData);
                setCurrentClass({ ...newClassData, id: existing.id });
                showMessage(`Clase "${newClassData.name}" actualizada.`, "success");
            } else {
                const ref = await addDoc(collection(db, `artifacts/${appId}/users/${userId}/classes`), newClassData);
                setCurrentClass({ ...newClassData, id: ref.id });
                showMessage(`Clase "${newClassData.name}" creada.`, "success");
            }
            setShowCreateClassModal(false);
            setClassNameInput('');
            setStudentsInput('');
        } catch (e) { showMessage("Error al guardar la clase.", "error"); }
    };

    const selectClass = (classId) => {
        const selected = savedClasses.find(cls => cls.id === classId);
        if (selected) {
            setCurrentClass(selected);
            showMessage(`Clase "${selected.name}" seleccionada.`, "success");
        }
    };
    
    const requestDelete = (type, item) => {
        let dependentActivities = [];
        const itemName = item.name || `${item.subject} - ${item.course}` || item.title;
    
        if (type === 'class') {
            dependentActivities = savedActivities.filter(activity => activity.classId === item.id);
        } else if (type === 'rubric') {
            dependentActivities = savedActivities.filter(activity => activity.rubricId === item.id);
        } else if (type === 'evalCriteria') {
            dependentActivities = savedActivities.filter(activity => activity.evalCriteriaId === item.id);
        }
    
        if (dependentActivities.length > 0) {
            setDependencyInfo({
                name: itemName,
                activities: dependentActivities
            });
            setShowDependencyModal(true);
        } else {
            // No dependencies, proceed with normal deletion confirmation
            setItemToDelete({ type, id: item.id, name: itemName });
            setShowDeleteConfirmModal(true);
        }
    };
    
    const openEditClassModal = (cls) => {
        setClassToEdit(cls);
        setShowEditClassModal(true);
    };

    const handleUpdateClass = async () => {
        if (!classToEdit) return;
        try {
            const classRef = doc(db, `artifacts/${appId}/users/${userId}/classes`, classToEdit.id);
            await updateDoc(classRef, { students: classToEdit.students });
            if (currentClass?.id === classToEdit.id) {
                setCurrentClass(classToEdit);
            }
            showMessage("Clase actualizada.", "success");
            setShowEditClassModal(false);
            setClassToEdit(null);
        } catch (e) { showMessage("Error al actualizar la clase.", "error"); }
    };

    const addStudentToEditList = () => {
        if (!newStudentName.trim() || !classToEdit) return;
        const newStudent = { id: `student-${Date.now()}`, name: newStudentName.trim() };
        setClassToEdit(prev => ({ ...prev, students: [...prev.students, newStudent] }));
        setNewStudentName("");
    };

    const removeStudentFromEditList = (studentId) => {
        if (!classToEdit) return;
        setClassToEdit(prev => ({ ...prev, students: prev.students.filter(s => s.id !== studentId) }));
    };

    // --- Rubric Management ---
    const handleRubricFileChange = (e) => {
        const file = e.target.files[0];
        if (!file) return;

        setRubricFileInput(file);

        if (!rubricNameInput.trim()) {
            const reader = new FileReader();
            reader.onload = (readEvent) => {
                const content = readEvent.target.result;
                const lines = content.split('\n').map(line => line.trim());
                let suggestedName = file.name.replace(/\.md$/i, '');
                if (!suggestedName && lines[0]?.startsWith('# ')) {
                    suggestedName = lines[0].substring(2).trim();
                }
                if (suggestedName) {
                    setRubricNameInput(suggestedName);
                }
            };
            reader.readAsText(file);
        }
    };

    const generateAndSaveRubricDescription = async (rubricId, rubricData) => {
        let promptText = `Basado en el nombre y los indicadores de la siguiente rúbrica, genera una descripción concisa de unos 250 caracteres que resuma qué evalúa. Responde únicamente con la descripción.\n\n`;
        promptText += `Nombre: ${rubricData.name}\n`;
        promptText += `Indicadores:\n`;
        rubricData.criteria.forEach(c => {
            const { name, weight } = parseCriterionNameAndWeight(c.name);
            promptText += `- ${name} ${weight ? `(${weight}%)` : ''}\n`;
        });
    
        try {
            const payload = { contents: [{ role: "user", parts: [{ text: promptText }] }] };
            const apiKey = "";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(url, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            if (!response.ok) throw new Error(`API Error: ${response.statusText}`);
            const result = await response.json();
            if (result.candidates && result.candidates.length > 0 && result.candidates[0].content && result.candidates[0].content.parts && result.candidates[0].content.parts.length > 0) {
                const description = result.candidates[0].content.parts[0].text.trim();
                const rubricRef = doc(db, `artifacts/${appId}/users/${userId}/rubrics`, rubricId);
                await updateDoc(rubricRef, { description });
            } else {
                console.error("Unexpected API response for description generation:", result);
                const rubricRef = doc(db, `artifacts/${appId}/users/${userId}/rubrics`, rubricId);
                await updateDoc(rubricRef, { description: "No se pudo generar la descripción." });
            }
        } catch (e) {
            console.error("Error generating rubric description:", e);
            const rubricRef = doc(db, `artifacts/${appId}/users/${userId}/rubrics`, rubricId);
            await updateDoc(rubricRef, { description: "Error al generar descripción." });
        }
    };

    const saveRubricToFirestore = async (rubricData) => {
        try {
            const dataToSave = {...rubricData, description: null };
            const ref = await addDoc(collection(db, `artifacts/${appId}/users/${userId}/rubrics`), dataToSave);
            const newRubric = { ...dataToSave, id: ref.id };
            setCurrentRubric(newRubric);
            setRubricNameInput(newRubric.name);
            showMessage(`Rúbrica "${rubricData.name}" cargada.`, "success");
            generateAndSaveRubricDescription(ref.id, rubricData);
        } catch (e) {
            showMessage("Error al guardar la rúbrica.", "error");
        }
    };

    const handleSaveRubric = () => {
        if (!rubricFileInput) return showMessage("Selecciona un archivo .md.", "error");
        if (!rubricNameInput.trim()) return showMessage("Introduce un nombre para la rúbrica.", "error");
        
        const reader = new FileReader();
        reader.onload = async (e) => {
            const content = e.target.result;
            const lines = content.split('\n').map(line => line.trim());
            const finalRubricName = rubricNameInput.trim();

            const cleanText = (text) => text.replace(/\*\*/g, '').trim();

            const sepIndex = lines.findIndex(line => line.match(/^\|?\s*:-+/));
            if (sepIndex === -1) return showMessage("Formato de tabla Markdown no válido.", "error");

            const headerParts = lines[sepIndex - 1].split('|').map(h => cleanText(h)).filter(Boolean);
            const levels = headerParts.slice(1);
            
            let totalWeight = 0;
            let hasWeights = false;

            const criteria = lines.slice(sepIndex + 1)
                .map((line, i) => {
                    const parts = line.split('|').map(p => cleanText(p)).filter(Boolean);
                    if (parts.length < 2) return null;
                    
                    const { weight } = parseCriterionNameAndWeight(parts[0]);
                    if (weight !== null) {
                        hasWeights = true;
                        totalWeight += weight;
                    }

                    return {
                        id: `criterion-${Date.now()}-${i}`,
                        name: `**${parts[0]}**`,
                        levels: parts.slice(1, levels.length + 1)
                    };
                }).filter(Boolean);
            
            if (criteria.length === 0 || levels.length === 0) return showMessage("No se pudieron extraer datos de la rúbrica.", "error");

            const newRubricData = { name: finalRubricName, levels: levels.map(l => `**${l}**`), criteria };

            if (hasWeights && totalWeight !== 100) {
                setValidationErrorInfo({
                    title: "Error en los Porcentajes",
                    message: `La suma de los indicadores es de ${totalWeight}%, es imprescindible que todos los indicadores sumen 100%. A continuación se abrirá la rúbrica en formato edición para que corrijas el error.`
                });
                setShowValidationErrorModal(true);
                openEditRubricModal({id: 'temp-id', ...newRubricData});
                return;
            }

            const existing = savedRubrics.find(r => r.name === finalRubricName);
            if (existing) {
                setPendingRubric(newRubricData);
                setNewRubricName(finalRubricName);
                setShowDuplicateRubricModal(true);
            } else {
                saveRubricToFirestore(newRubricData);
            }
        };
        reader.readAsText(rubricFileInput);
    };

    const handleSaveWithNewName = async () => {
        if (!pendingRubric || !newRubricName.trim()) return;

        const alreadyExists = savedRubrics.find(r => r.name === newRubricName.trim());
        if(alreadyExists) {
            return showMessage("Ese nombre ya existe. Por favor, elige otro.", "error");
        }

        const rubricToSave = { ...pendingRubric, name: newRubricName.trim() };
        await saveRubricToFirestore(rubricToSave);
        
        setShowDuplicateRubricModal(false);
        setPendingRubric(null);
        setNewRubricName("");
    };
    
    const selectRubric = (rubricId) => {
        const selected = savedRubrics.find(rub => rub.id === rubricId);
        if (selected) {
            setCurrentRubric(selected);
            setRubricNameInput(selected.name);
            showMessage(`Rúbrica "${selected.name}" seleccionada.`, "success");
        }
    };
    
    const openEditRubricModal = (rubric) => {
        const editableData = JSON.parse(JSON.stringify(rubric));
        editableData.criteria = editableData.criteria.map(c => {
            const { name, weight } = parseCriterionNameAndWeight(c.name);
            return { ...c, cleanName: name, weight: weight };
        });
        setRubricToEdit(rubric);
        setEditableRubricData(editableData);
        setShowEditRubricModal(true);
    };

    const handleRubricDataChange = (critIdx, field, value) => {
        setEditableRubricData(prev => {
            const newCriteria = [...prev.criteria];
            const currentCriterion = { ...newCriteria[critIdx] };
    
            if (field === 'name') {
                currentCriterion.cleanName = value;
            } else if (field === 'weight') {
                currentCriterion.weight = value === '' ? null : Number(value);
            }
    
            newCriteria[critIdx] = currentCriterion;
            return { ...prev, criteria: newCriteria };
        });
    };
    
    const handleUpdateRubric = async () => {
        if (!editableRubricData) return;
    
        const { criteria } = editableRubricData;
        const weights = criteria.map(c => c.weight);
        const hasWeights = weights.some(w => w !== null);
    
        if (hasWeights) {
            const totalWeight = weights.reduce((sum, w) => sum + (w || 0), 0);
            if (totalWeight !== 100) {
                setValidationErrorInfo({
                    title: "Error de Validación",
                    message: `Los indicadores no suman 100%, actualmente suman ${totalWeight}%. Por favor, edite los valores.`
                });
                setShowValidationErrorModal(true);
                return;
            }
        }
    
        // Reconstruct the name string before saving
        const criteriaToSave = editableRubricData.criteria.map(c => {
            const newNameString = c.weight !== null ? `**${c.cleanName} (${c.weight}%)**` : `**${c.cleanName}**`;
            const { cleanName, weight, ...rest } = c; // Remove temporary fields
            return { ...rest, name: newNameString };
        });
    
        const dataToSave = {
            name: editableRubricData.name,
            levels: editableRubricData.levels,
            criteria: criteriaToSave,
        };

        try {
            if (editableRubricData.id === 'temp-id') {
                await saveRubricToFirestore(dataToSave);
            } else {
                const rubricRef = doc(db, `artifacts/${appId}/users/${userId}/rubrics`, editableRubricData.id);
                await updateDoc(rubricRef, dataToSave);
                const updatedRubric = {id: editableRubricData.id, ...dataToSave};
                setCurrentRubric(updatedRubric);
                setRubricNameInput(updatedRubric.name);
                showMessage("Rúbrica actualizada.", "success");
                generateAndSaveRubricDescription(editableRubricData.id, dataToSave);
            }
            
            setShowEditRubricModal(false);
            setEditableRubricData(null);
        } catch (e) { 
            console.error("Error saving/updating rubric:", e);
            showMessage("Error al guardar la rúbrica.", "error"); 
        }
    };

    // --- Evaluation Criteria Management ---
    const openCreateEvalCriteriaModal = () => {
        setCriteriaToEdit(null);
        setNewCriteriaSubject('');
        setNewCriteriaCourse('');
        setTableCriteria([{ id: Date.now(), code: '', text: '' }]);
        setShowCreateEvalCriteriaModal(true);
    };
    
    const openEditEvalCriteriaModal = (criteriaSet) => {
        setCriteriaToEdit(criteriaSet);
        setNewCriteriaSubject(criteriaSet.subject);
        setNewCriteriaCourse(criteriaSet.course);
        setTableCriteria(criteriaSet.criteria.map((c, i) => ({...c, id: `crit-${Date.now()}-${i}`})));
        setShowCreateEvalCriteriaModal(true);
    };

    const addCriteriaRow = () => {
        setTableCriteria(prev => [...prev, { id: Date.now(), code: '', text: '' }]);
    };

    const removeCriteriaRow = (id) => {
        setTableCriteria(prev => prev.filter(row => row.id !== id));
    };

    const handleCriteriaRowChange = (id, field, value) => {
        setTableCriteria(prev => prev.map(row => row.id === id ? { ...row, [field]: value } : row));
    };

    const handleSaveEvalCriteria = async () => {
        if (!newCriteriaSubject.trim() || !newCriteriaCourse.trim()) {
            return showMessage("Asignatura y curso son obligatorios.", "error");
        }
        const criteria = tableCriteria.filter(c => c.code.trim() && c.text.trim());
        if (criteria.length === 0) {
            return showMessage("Añade al menos un criterio válido.", "error");
        }

        const newCriteriaSet = {
            subject: newCriteriaSubject.trim(),
            course: newCriteriaCourse.trim(),
            criteria: criteria.map(({code, text}) => ({code, text}))
        };

        try {
            if (criteriaToEdit) {
                const docRef = doc(db, `artifacts/${appId}/users/${userId}/evalCriteria`, criteriaToEdit.id);
                await updateDoc(docRef, newCriteriaSet);
                showMessage("Criterios actualizados.", "success");
            } else {
                await addDoc(collection(db, `artifacts/${appId}/users/${userId}/evalCriteria`), newCriteriaSet);
                showMessage("Criterios guardados.", "success");
            }
            setShowCreateEvalCriteriaModal(false);
            setCriteriaToEdit(null);
        } catch (e) {
            console.error(e);
            showMessage("Error al guardar los criterios.", "error");
        }
    };

    const handleProcessAndSaveImportedCriteria = () => {
        if (!criteriaMdFile) {
            return showMessage("Por favor, selecciona un archivo .md para procesar.", "error");
        }
        if (!newCriteriaSubject.trim() || !newCriteriaCourse.trim()) {
            return showMessage("Asignatura y curso son obligatorios para guardar.", "error");
        }

        const reader = new FileReader();
        reader.onload = async (e) => {
            const content = e.target.result;
            const lines = content.split('\n').map(line => line.trim());
            const separatorIndex = lines.findIndex(line => line.match(/^\|?\s*:-/));

            if (separatorIndex === -1) {
                return showMessage("No se encontró una tabla de Markdown válida en el archivo.", "error");
            }

            const dataLines = lines.slice(separatorIndex + 1);
            const importedCriteria = dataLines.map(line => {
                const parts = line.split('|').map(p => p.trim()).filter(p => p.length > 0);
                if (parts.length >= 2) {
                    return { code: parts[0], text: parts[1] };
                }
                return null;
            }).filter(Boolean);

            if (importedCriteria.length === 0) {
                return showMessage("No se encontraron filas de criterios válidas en la tabla.", "error");
            }

            const newCriteriaSet = {
                subject: newCriteriaSubject.trim(),
                course: newCriteriaCourse.trim(),
                criteria: importedCriteria
            };

            try {
                if (criteriaToEdit) {
                    const docRef = doc(db, `artifacts/${appId}/users/${userId}/evalCriteria`, criteriaToEdit.id);
                    await updateDoc(docRef, newCriteriaSet);
                    showMessage("Lote de criterios actualizado con éxito desde el archivo.", "success");
                } else {
                    await addDoc(collection(db, `artifacts/${appId}/users/${userId}/evalCriteria`), newCriteriaSet);
                    showMessage("Nuevo lote de criterios importado y guardado con éxito.", "success");
                }
                
                setShowImportCriteriaModal(false);
                setShowCreateEvalCriteriaModal(false); 
                setCriteriaMdFile(null);

            } catch (error) {
                console.error("Error saving imported criteria:", error);
                showMessage("Error al guardar los criterios importados.", "error");
            }
        };
        reader.onerror = () => showMessage("Error al leer el archivo.", "error");
        reader.readAsText(criteriaMdFile);
    };
    
    const handleConfirmDelete = async () => {
        if (!itemToDelete) return;
        const { type, id, name } = itemToDelete;
        
        let collectionName;
        if (type === 'evalCriteria') {
            collectionName = 'evalCriteria';
        } else if (type === 'activity') {
            collectionName = 'activities';
        } else if (type === 'class') {
            collectionName = 'classes';
        } else {
            collectionName = `${type}s`;
        }
    
        try {
            if (type === 'activity') {
                const evalDocRef = doc(db, `artifacts/${appId}/users/${userId}/evaluations`, id);
                await deleteDoc(evalDocRef).catch(err => {
                    if (err.code !== 'not-found') {
                        throw err;
                    }
                });
            }
    
            await deleteDoc(doc(db, `artifacts/${appId}/users/${userId}/${collectionName}`, id));
            
            if (type === 'class' && currentClass?.id === id) {
                setCurrentClass(null);
            } else if (type === 'rubric' && currentRubric?.id === id) {
                setCurrentRubric(null);
            }
            showMessage(`${type.charAt(0).toUpperCase() + type.slice(1)} "${name}" eliminado.`, "success");
        } catch (e) {
            console.error("Deletion error:", e);
            showMessage(`Error al eliminar.`, "error");
        } finally {
            setShowDeleteConfirmModal(false);
            setItemToDelete(null);
        }
    };

    // --- Evaluation & Report Logic ---
    const handleRequestCloseEvalModal = () => {
        if (!selectedStudent) {
            setShowEvalModal(false);
            return;
        }
        const studentEvals = tempEvaluation;
        if (studentEvals && Object.keys(studentEvals).length > 0) {
            setShowCloseEvalConfirmModal(true);
        } else {
            setShowEvalModal(false);
        }
    };

    const openEvaluationModal = (student) => {
        if (!currentRubric) return showMessage("Carga una rúbrica antes de evaluar.", "error");
        setSelectedStudent(student);
        setTempEvaluation(evaluations[student.id] || {});
        setLlmFeedback('');
        setShowEvalModal(true);
    };

    const handleCellClick = (criterionId, levelIndex) => {
        if (!selectedStudent) return;
        setTempEvaluation(prev => {
            const newEvals = { ...prev };
            if (newEvals[criterionId] === levelIndex) {
                delete newEvals[criterionId];
            } else {
                newEvals[criterionId] = levelIndex;
            }
            return newEvals;
        });
        setLlmFeedback('');
    };

    const handleSaveEvaluation = () => {
        if (!selectedStudent) return;
        setEvaluations(prev => ({
            ...prev,
            [selectedStudent.id]: tempEvaluation
        }));
        setShowEvalModal(false);
    };

    const calculateStudentScore = (studentId, evalData) => {
        const studentEval = evalData || evaluations[studentId];
        if (!currentRubric || !studentEval) return null;
    
        const { criteria, levels } = currentRubric;
        const numLevels = levels.length;
    
        if (criteria.length === 0 || numLevels === 0 || Object.keys(studentEval).length === 0) return null;
    
        const weights = criteria.map(c => parseCriterionNameAndWeight(c.name).weight);
        const hasWeights = weights.some(w => w !== null);
    
        const scoreStep = 10 / numLevels;
    
        if (hasWeights) {
            let totalWeightedScore = 0;
            let totalWeightOfEvaluatedCriteria = 0;
    
            criteria.forEach(c => {
                const levelIndex = studentEval[c.id];
                if (levelIndex !== undefined) {
                    const weight = parseCriterionNameAndWeight(c.name).weight || 0;
                    const criterionScore = 10 - (levelIndex * scoreStep);
                    totalWeightedScore += (criterionScore / 10) * weight;
                    totalWeightOfEvaluatedCriteria += weight;
                }
            });
    
            if (totalWeightOfEvaluatedCriteria === 0) return null;
            return (totalWeightedScore / totalWeightOfEvaluatedCriteria) * 10;
    
        } else {
            let totalScore = 0;
            let evaluatedCount = 0;
    
            criteria.forEach(c => {
                const levelIndex = studentEval[c.id];
                if (levelIndex !== undefined) {
                    const criterionScore = 10 - (levelIndex * scoreStep);
                    totalScore += criterionScore;
                    evaluatedCount++;
                }
            });
    
            if (evaluatedCount === 0) return null;
            return totalScore / evaluatedCount;
        }
    };
    
    const generateReportPreview = () => {
        if (!currentRubric || !currentClass) return showMessage("Selecciona una clase y una rúbrica.", "error");
        
        const currentActivity = savedActivities.find(a => a.id === currentActivityId);

        setReportPreviewData({
            rubric: currentRubric,
            class: currentClass,
            evaluations: evaluations,
            calculateStudentScore: calculateStudentScore,
            activityTitle: activityTitle,
            selectedCriteria: currentActivity?.selectedCriteria || [],
        });
        setCurrentPage('reportPreview');
    };
    
    const handleStartEvaluation = async () => {
        if (!activityTitleInput.trim()) {
            return showMessage("Por favor, introduce un título para la actividad.", "error");
        }
        if (!currentClass || !currentRubric) {
            return showMessage("Selecciona una clase y una rúbrica activas primero.", "error");
        }
        
        const newActivity = {
            title: activityTitleInput.trim(),
            classId: currentClass.id,
            rubricId: currentRubric.id,
            evalCriteriaId: criteriaSetToAssign, 
            selectedCriteria: selectedCriteriaForActivity,
            createdAt: serverTimestamp(),
            lastModified: serverTimestamp()
        };
        try {
            const docRef = await addDoc(collection(db, `artifacts/${appId}/users/${userId}/activities`), newActivity);
            setActivityTitle(newActivity.title);
            setCurrentActivityId(docRef.id);
            setShowActivityTitleModal(false);
            setCurrentPage('evaluation');
        } catch (e) {
            showMessage("Error al crear la actividad.", "error");
        }
    };

    const handleOpenActivity = (activity) => {
        const classForActivity = savedClasses.find(c => c.id === activity.classId);
        const rubricForActivity = savedRubrics.find(r => r.id === activity.rubricId);
        if (!classForActivity || !rubricForActivity) {
            return showMessage("No se pudo encontrar la clase o la rúbrica para esta actividad.", "error");
        }
        setCurrentClass(classForActivity);
        setCurrentRubric(rubricForActivity);
        setActivityTitle(activity.title);
        setCurrentActivityId(activity.id);
        setCurrentPage('evaluation');
    };
    
    const handleSaveAndNext = () => {
        handleSaveEvaluation();
        
        const currentIndex = currentClass.students.findIndex(s => s.id === selectedStudent.id);
        const nextIndex = currentIndex + 1;

        if (nextIndex < currentClass.students.length) {
            const nextStudent = currentClass.students[nextIndex];
            setTimeout(() => openEvaluationModal(nextStudent), 50);
        } else {
            showMessage("Has evaluado al último estudiante de la lista.", "success");
        }
    };

    const exportPdf = () => {
        if (!reportPreviewData || !isJsPDFLoaded) return;
        const { rubric, class: classData, evaluations: allEvaluations, calculateStudentScore: scoreCalculator, activityTitle, selectedCriteria } = reportPreviewData;
        const { jsPDF } = window.jspdf;
        const doc = new jsPDF({ orientation: "portrait", unit: "mm", format: "a4" });
        
        const cleanTextForPdf = (text) => text ? text.replace(/\*\*/g, '') : '';

        const sanitizedActivityTitle = cleanTextForPdf(activityTitle);
        const sanitizedClassName = cleanTextForPdf(classData.name);

        doc.setFont("Helvetica");

        let yOffset = 20;
        doc.setFontSize(18);
        doc.text(`Evaluación de la actividad: ${sanitizedActivityTitle}`, doc.internal.pageSize.getWidth() / 2, yOffset, { align: 'center' });
        yOffset += 10;
        
        doc.setFontSize(12);
        doc.text(`Clase: ${sanitizedClassName}`, 14, yOffset);
        yOffset += 12;
        
        if (selectedCriteria && selectedCriteria.length > 0) {
             doc.setFontSize(10);
             doc.text("Criterios de Evaluación Aplicados", 14, yOffset);
             yOffset += 6;
             const criteriaHead = [['Código', 'Descripción']];
             const criteriaBody = selectedCriteria.map(c => [c.code, c.text]);
             doc.autoTable({
                 startY: yOffset,
                 head: criteriaHead,
                 body: criteriaBody,
                 theme: 'striped',
                 headStyles: { fillColor: [150, 150, 150], textColor: 255, fontStyle: 'bold' },
                 didDrawPage: (data) => { yOffset = data.cursor.y; }
             });
             yOffset = doc.autoTable.previous.finalY + 15;
        }

        doc.setFontSize(10);
        doc.text("Rúbrica Completa", 14, yOffset);
        yOffset += 6;
        const fullRubricHead = [['Indicador', ...rubric.levels.map(cleanTextForPdf)]];
        const fullRubricBody = rubric.criteria.map(criterion => [cleanTextForPdf(criterion.name), ...criterion.levels.map(cleanTextForPdf)]);
        doc.autoTable({
            startY: yOffset,
            head: fullRubricHead,
            body: fullRubricBody,
            theme: 'grid',
            headStyles: { fillColor: [220, 220, 220], textColor: 0, fontStyle: 'bold' },
            didDrawPage: (data) => { yOffset = data.cursor.y; }
        });
        yOffset = doc.autoTable.previous.finalY + 15;

        if (yOffset > 250) { doc.addPage(); yOffset = 20; }
        doc.setFontSize(10);
        doc.text("Puntuaciones Generales", 14, yOffset);
        yOffset += 6;
        const summaryHead = [['Estudiante', 'Puntuación (/10)']];
        const summaryBody = classData.students.map(s => {
            const score = scoreCalculator(s.id);
            const scoreDisplay = score === null ? 'Pendiente' : score.toFixed(2);
            const cellStyle = score !== null && score < 5 ? { fillColor: [220, 53, 69], textColor: [255, 255, 255] } : {};
            return [s.name, { content: scoreDisplay, styles: cellStyle }];
        });
        doc.autoTable({ startY: yOffset, head: summaryHead, body: summaryBody, didDrawPage: (data) => { yOffset = data.cursor.y; } });
        yOffset = doc.autoTable.previous.finalY + 15;

        classData.students.forEach((student) => {
            const studentEval = allEvaluations[student.id] || {};
            const estimatedHeight = 15 + (rubric.criteria.length * 8);
            if (yOffset + estimatedHeight > doc.internal.pageSize.getHeight() - 20) {
                doc.addPage();
                yOffset = 20;
            }

            doc.setFontSize(12);
            doc.text(`Evaluación para: ${student.name}`, 14, yOffset);
            yOffset += 8;
            
            const head = [['Indicador', ...rubric.levels.map(cleanTextForPdf)]];
            const body = rubric.criteria.map(criterion => {
                const row = [{ content: cleanTextForPdf(criterion.name), styles: { fontStyle: 'bold' } }];
                criterion.levels.forEach((level, index) => {
                    const isSelected = studentEval[criterion.id] === index;
                    row.push({
                        content: '', 
                        styles: { fillColor: isSelected ? [255, 229, 204] : [255, 255, 255] }
                    });
                });
                return row;
            });
            doc.autoTable({ startY: yOffset, head: head, body: body, theme: 'grid', didDrawPage: (data) => { yOffset = data.cursor.y; } });
            yOffset = doc.autoTable.previous.finalY + 15;
        });

        doc.save(`informe_${classData.name}.pdf`);
        showMessage("Informe PDF generado.", "success");
    };

    // --- LLM Feedback ---
    const generateStudentFeedback = async () => {
        if (!selectedStudent || !currentRubric || !tempEvaluation) return;
        setIsGeneratingFeedback(true);
        setLlmFeedback('');
        let promptDetails = `Estudiante: ${selectedStudent.name}\nResultados:\n`;
        currentRubric.criteria.forEach(c => {
            const levelIndex = tempEvaluation?.[c.id];
            promptDetails += `- ${c.name}: ${levelIndex !== undefined ? c.levels[levelIndex] : 'No evaluado'}\n`;
        });
        const prompt = `Genera feedback constructivo y positivo para el estudiante, basándote en los resultados de la rúbrica. Enfócate en puntos fuertes y áreas de mejora. Sé breve y alentador. **Enfatiza solo las palabras clave más importantes en negrita (usando **palabra**), no la frase entera.** Idioma: Español.\n${promptDetails}`;

        try {
            const payload = { contents: [{ role: "user", parts: [{ text: prompt }] }] };
            const apiKey = "";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(url, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            if (!response.ok) throw new Error(`API Error: ${response.statusText}`);
            const result = await response.json();
            setLlmFeedback(result.candidates[0].content.parts[0].text);
        } catch (e) {
            setLlmFeedback("No se pudo generar el feedback.");
        } finally {
            setIsGeneratingFeedback(false);
        }
    };
    
    const parseMarkdown = (text, showWeight = true) => {
        if (!text) return null;
        const { name, weight } = parseCriterionNameAndWeight(text);
        let html = `<strong>${name}</strong>`;
        if (showWeight && weight !== null) {
            html += `<div class="mt-1"><span class="bg-green-600 text-white text-xs font-semibold px-2 py-0.5 rounded-full">${weight}%</span></div>`;
        }
        return (
            <div dangerouslySetInnerHTML={{ __html: html }} />
        );
    };
    
    const handleSuggestCriteria = async () => {
        if (!activityTitleInput || !currentRubric || !criteriaSetToAssign) {
            showMessage("Por favor, introduce un título, selecciona una rúbrica y un lote de criterios.", "error");
            return;
        }

        const criteriaSet = savedEvalCriteria.find(s => s.id === criteriaSetToAssign);
        if (!criteriaSet || criteriaSet.criteria.length === 0) {
            showMessage("El lote de criterios seleccionado no tiene criterios disponibles.", "error");
            return;
        }

        setIsSuggestingCriteria(true);
        let prompt = `Analiza el título de la actividad y la rúbrica para determinar cuáles de los siguientes criterios de evaluación son los más relevantes. Devuelve un máximo de 4.
        
        Título de la actividad: "${activityTitleInput}"
        
        Rúbrica: "${currentRubric.name}"
        Criterios de la rúbrica: ${currentRubric.criteria.map(c => parseCriterionNameAndWeight(c.name).name).join(', ')}
        
        Lista de Criterios de Evaluación Disponibles (con sus códigos):
        ${criteriaSet.criteria.map(c => `- Código: ${c.code}, Descripción: ${c.text}`).join('\n')}
        
        Basándote en la relación semántica, devuelve únicamente un array JSON con los 'codes' de los 4 criterios más relevantes. Ejemplo de respuesta: ["1.1", "2.3", "4.1"]`;
    
        try {
            const payload = { 
                contents: [{ role: "user", parts: [{ text: prompt }] }],
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "ARRAY",
                        items: {
                            type: "STRING"
                        }
                    }
                }
            };
            const apiKey = "";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(url, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });

            if (!response.ok) throw new Error(`API Error: ${response.statusText}`);
            
            const result = await response.json();
            const suggestedCodesText = result.candidates[0].content.parts[0].text;
            const suggestedCodes = JSON.parse(suggestedCodesText);
            
            const suggestedCriteria = criteriaSet.criteria.filter(c => suggestedCodes.includes(c.code));
            setSelectedCriteriaForActivity(suggestedCriteria);
            showMessage("Criterios sugeridos aplicados.", "success");

        } catch (error) {
            console.error("Error suggesting criteria:", error);
            showMessage("No se pudieron sugerir los criterios.", "error");
        } finally {
            setIsSuggestingCriteria(false);
        }
    };

    const handleGenerateRubricByAI = async () => {
        if (!generateRubricTitle || !generateRubricDesc || !generateRubricCriteriaSet || generateRubricSelectedCriteria.length === 0) {
            showMessage("Por favor, completa todos los campos para generar la rúbrica.", "error");
            return;
        }
        setIsGeneratingRubric(true);

        const criteriaToProcess = generateRubricSelectedCriteria.map(c => `- ${c.code}: ${c.text}`).join('\n');
        
        const prompt = `Basado en la siguiente información, crea una rúbrica.
        
        Título de la actividad a evaluar: "${generateRubricDesc}"
        
        Criterios de evaluación seleccionados:
        ${criteriaToProcess}
        
        Para cada uno de los criterios de evaluación, genera una descripción para 4 niveles de desempeño: Excelente, Muy Bien, Aceptable, Pobre.
        
        Responde únicamente con un objeto JSON que contenga una clave "criteria". El valor de "criteria" debe ser un array de objetos. Cada objeto debe tener una clave "criterion_name" (con el texto completo del criterio, ej: "1.1. Comprende el problema") y una clave "levels" que sea un array de 4 strings, una para cada nivel (Excelente, Muy Bien, Aceptable, Pobre).
        
        Ejemplo de formato de respuesta:
        {
          "criteria": [
            {
              "criterion_name": "1.1. Comprende el problema",
              "levels": [
                "Descripción para Excelente...",
                "Descripción para Muy Bien...",
                "Descripción para Aceptable...",
                "Descripción para Pobre..."
              ]
            }
          ]
        }`;

        try {
            const payload = { 
                contents: [{ role: "user", parts: [{ text: prompt }] }],
                generationConfig: {
                    responseMimeType: "application/json",
                }
            };
            const apiKey = "";
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;
            const response = await fetch(url, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });

            if (!response.ok) throw new Error(`API Error: ${response.statusText}`);
            
            const result = await response.json();
            const generatedDataText = result.candidates[0].content.parts[0].text;
            const generatedData = JSON.parse(generatedDataText);

            const newRubricCriteria = generatedData.criteria.map((item, index) => ({
                id: `criterion-${Date.now()}-${index}`,
                name: `**${item.criterion_name} (0%)**`,
                levels: item.levels
            }));

            const newRubric = {
                id: 'temp-id',
                name: generateRubricTitle,
                levels: ['**Excelente**', '**Muy Bien**', '**Aceptable**', '**Pobre**'],
                criteria: newRubricCriteria
            };

            setShowGenerateRubricModal(false);
            openEditRubricModal(newRubric);

        } catch (error) {
            console.error("Error generating rubric by AI:", error);
            showMessage("No se pudo generar la rúbrica.", "error");
        } finally {
            setIsGeneratingRubric(false);
        }
    };


    // --- Render Functions ---
    if (!isAuthReady || !db) {
        return <div className="flex items-center justify-center h-screen bg-gray-100"><Spinner /></div>;
    }

    const renderMainPage = () => {
        const filteredActivities = savedActivities.filter(activity => {
            const titleMatch = activity.title.toLowerCase().includes(activitySearchTerm.toLowerCase());
            const classMatch = !selectedClassFilter || activity.classId === selectedClassFilter;
            const rubricMatch = !selectedRubricFilter || activity.rubricId === selectedRubricFilter;
            return titleMatch && classMatch && rubricMatch;
        });

        return (
            <>
                <header className="bg-white shadow-sm p-4 sticky top-0 z-10">
                    <h1 className="text-2xl font-bold text-gray-800 text-center">Herramienta de Evaluación con Rúbricas</h1>
                </header>
                <main className="p-4 sm:p-8">
                    <div className="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
                        {/* Class Management */}
                        <div className="bg-white p-6 rounded-2xl shadow-lg space-y-4">
                            <h2 className="text-xl font-bold border-b pb-2">Gestionar Clases</h2>
                            <div className="pt-2">
                                {savedClasses.length > 0 && <h3 className="font-semibold">Clases Guardadas:</h3>}
                                <ul className="max-h-48 overflow-y-auto space-y-2 mt-2">
                                    {savedClasses.map(cls => (
                                        <li key={cls.id} className={`flex justify-between items-center p-2 rounded-lg ${currentClass?.id === cls.id ? 'bg-blue-100' : 'bg-gray-100'}`}>
                                            <span className="font-medium">{cls.name} <span className="text-sm text-gray-500">({cls.students.length} est.)</span></span>
                                            <div className="flex items-center space-x-1">
                                                <button onClick={() => selectClass(cls.id)} className="text-xs bg-green-500 text-white py-1 px-2 rounded hover:bg-green-600">Usar</button>
                                                <button onClick={() => openEditClassModal(cls)} className="text-xs bg-yellow-500 text-white py-1 px-2 rounded hover:bg-yellow-600">Editar</button>
                                                <button onClick={() => requestDelete('class', cls)} className="text-xs bg-red-500 text-white py-1 px-2 rounded hover:bg-red-600">X</button>
                                            </div>
                                        </li>
                                    ))}
                                </ul>
                            </div>
                            <button onClick={() => setShowCreateClassModal(true)} className="w-full bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Crear Nueva Clase</button>
                        </div>
                        {/* Rubric Management */}
                        <div className="bg-white p-6 rounded-2xl shadow-lg space-y-4">
                            <h2 className="text-xl font-bold border-b pb-2">Gestionar Rúbricas</h2>
                            <input type="text" value={rubricNameInput} onChange={e => setRubricNameInput(e.target.value)} placeholder="Nombre de la rúbrica" className="w-full p-2 border rounded-lg" />
                            <div className="flex items-center space-x-2">
                                <button onClick={() => setShowInfoModal(true)} className="p-2 rounded-full hover:bg-gray-200 flex-shrink-0" title="Información sobre formato">
                                    <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 text-gray-600" viewBox="0 0 20 20" fill="currentColor">
                                        <path fillRule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clipRule="evenodd" />
                                    </svg>
                                </button>
                                <label className="bg-blue-500 text-white text-center font-bold py-2 px-4 rounded-lg hover:bg-blue-600 cursor-pointer flex-shrink-0">
                                    Seleccionar archivo md
                                    <input type="file" onChange={handleRubricFileChange} accept=".md" className="hidden"/>
                                </label>
                                 <span className="text-sm text-gray-500 truncate flex-grow text-left">
                                    {rubricFileInput?.name || "Ningún archivo seleccionado"}
                                </span>
                            </div>
                            <button onClick={handleSaveRubric} className="w-full bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Cargar Rúbrica</button>
                            <div className="pt-2">
                               {savedRubrics.length > 0 && <h3 className="font-semibold">Rúbricas Guardadas:</h3>}
                                <ul className="max-h-40 overflow-y-auto space-y-2 mt-2">
                                    {savedRubrics.map(rub => (
                                        <li key={rub.id} className={`flex justify-between items-center p-2 rounded-lg ${currentRubric?.id === rub.id ? 'bg-blue-100' : 'bg-gray-100'}`}>
                                            <div>
                                                <span className="font-medium">{rub.name}</span>
                                                {rub.description ? (
                                                    <p className="text-xs text-gray-500 mt-1">{rub.description}</p>
                                                ) : (
                                                    <p className="text-xs text-yellow-600 italic mt-1">Pendiente de descripción automática por IA</p>
                                                )}
                                            </div>
                                            <div className="flex items-center space-x-1 flex-shrink-0">
                                                <button onClick={() => selectRubric(rub.id)} className="text-xs bg-green-500 text-white py-1 px-2 rounded hover:bg-green-600">Usar</button>
                                                <button onClick={() => openEditRubricModal(rub)} className="text-xs bg-yellow-500 text-white py-1 px-2 rounded hover:bg-yellow-600">Editar</button>
                                                <button onClick={() => requestDelete('rubric', rub)} className="text-xs bg-red-500 text-white py-1 px-2 rounded hover:bg-red-600">X</button>
                                            </div>
                                        </li>
                                    ))}
                                </ul>
                            </div>
                        </div>
                    </div>

                    {/* Evaluation Criteria Management */}
                    <div className="bg-white p-6 rounded-2xl shadow-lg space-y-4 mt-8">
                        <div className="flex items-center space-x-3 border-b pb-2">
                            <h2 className="text-xl font-bold">Gestionar Criterios de Evaluación</h2>
                            <span className="bg-yellow-400 text-white text-xs font-bold px-2 py-1 rounded-full">OPCIONAL</span>
                        </div>
                        
                        {savedEvalCriteria.length > 0 ? (
                            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 pt-2">
                                {savedEvalCriteria.map(ec => (
                                    <div key={ec.id} className="bg-gray-50 p-4 rounded-lg shadow-sm flex flex-col justify-between">
                                        <div>
                                            <h4 className="font-bold text-gray-800">{ec.subject} ({ec.course})</h4>
                                            <p className="text-xs text-gray-500 mt-1">{ec.criteria.length} criterios de evaluación</p>
                                        </div>
                                        <div className="flex items-center space-x-2 mt-4">
                                            <button onClick={() => openEditEvalCriteriaModal(ec)} className="w-full text-sm bg-yellow-500 text-white py-1 px-2 rounded hover:bg-yellow-600">Editar</button>
                                            <button onClick={() => requestDelete('evalCriteria', ec)} className="w-full text-sm bg-red-500 text-white py-1 px-2 rounded hover:bg-red-600">Eliminar</button>
                                        </div>
                                    </div>
                                ))}
                            </div>
                        ) : (
                            <p className="text-center text-gray-500 py-4">No hay criterios guardados.</p>
                        )}

                        <button onClick={openCreateEvalCriteriaModal} className="w-full bg-purple-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-purple-700">Crear Nuevo Lote de Criterios</button>
                    </div>

                    <div className="text-center mt-8">
                        <button 
                            onClick={() => { setActivityTitleInput(""); setCriteriaSetToAssign(''); setSelectedCriteriaForActivity([]); setShowActivityTitleModal(true); }}
                            disabled={!currentClass || !currentRubric}
                            className="bg-teal-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-teal-700 disabled:bg-gray-400 disabled:cursor-not-allowed transition-transform transform hover:scale-105"
                        >
                            Crear Actividad de Evaluación
                        </button>
                         {!currentClass && <p className="text-sm text-red-500 mt-2">Selecciona una clase para empezar.</p>}
                         {!currentRubric && <p className="text-sm text-red-500 mt-2">Selecciona una rúbrica para empezar.</p>}
                    </div>

                    {/* Activities Section */}
                    {savedActivities.length > 0 && (
                         <div className="mt-12">
                            <h2 className="text-xl font-bold border-b pb-2 mb-4">Historial de Actividades</h2>
                            <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4 bg-gray-100 p-4 rounded-lg">
                                <input 
                                    type="text"
                                    placeholder="Buscar por título..."
                                    value={activitySearchTerm}
                                    onChange={e => setActivitySearchTerm(e.target.value)}
                                    className="p-2 border rounded-lg"
                                />
                                <select value={selectedClassFilter} onChange={e => setSelectedClassFilter(e.target.value)} className="p-2 border rounded-lg">
                                    <option value="">Todas las clases</option>
                                    {savedClasses.map(c => <option key={c.id} value={c.id}>{c.name}</option>)}
                                </select>
                                <select value={selectedRubricFilter} onChange={e => setSelectedRubricFilter(e.target.value)} className="p-2 border rounded-lg">
                                    <option value="">Todas las rúbricas</option>
                                    {savedRubrics.map(r => <option key={r.id} value={r.id}>{r.name}</option>)}
                                </select>
                            </div>
                            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                                {filteredActivities.map(activity => {
                                    const className = savedClasses.find(c => c.id === activity.classId)?.name || 'Clase no encontrada';
                                    const rubricName = savedRubrics.find(r => r.id === activity.rubricId)?.name || 'Rúbrica no encontrada';
                                    return (
                                        <div key={activity.id} className="bg-white p-4 rounded-lg shadow flex flex-col justify-between">
                                            <div>
                                                <h4 className="font-bold text-lg mb-2">{activity.title}</h4>
                                                <p className="text-sm text-gray-600">Clase: {className}</p>
                                                <p className="text-sm text-gray-600">Rúbrica: {rubricName}</p>
                                                <p className="text-xs text-gray-500 mt-2">Modificado: {formatDate(activity.lastModified)}</p>
                                            </div>
                                            <div className="flex items-center space-x-2 mt-4">
                                                <button onClick={() => handleOpenActivity(activity)} className="w-full bg-green-600 text-white font-semibold py-2 rounded-lg hover:bg-green-700">
                                                    Abrir
                                                </button>
                                                <button onClick={() => requestDelete('activity', activity)} className="bg-red-600 text-white p-2 rounded-lg hover:bg-red-700">
                                                    <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                                                        <path fillRule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm4 0a1 1 0 012 0v6a1 1 0 11-2 0V8z" clipRule="evenodd" />
                                                    </svg>
                                                </button>
                                            </div>
                                        </div>
                                    )
                                })}
                            </div>
                        </div>
                    )}
                </main>
                <footer className="text-center p-4 text-xs text-gray-500">
                    © 2025 por Eduardo Ruiz (@ejruizgarcia). Esta app ha sido desarrollada utilizando Gemini y tiene licencia CC BY-NC 4.0.
                </footer>
            </>
        );
    }
    const renderEvaluationPage = () => {
        const currentActivity = savedActivities.find(a => a.id === currentActivityId);
        const criteriaSetInfo = savedEvalCriteria.find(c => c.id === currentActivity?.evalCriteriaId);

        return (
            <div className="p-4 sm:p-8">
                <header className="flex justify-between items-start mb-6 flex-wrap">
                    <div className="mb-4">
                        <h1 className="text-3xl font-bold text-gray-800">Evaluación de Estudiantes</h1>
                        <p className="text-gray-600 max-w-xl">
                            Actividad: <strong>{activityTitle}</strong> | Clase: <strong>{currentClass?.name}</strong> | Rúbrica: <strong>{currentRubric?.name}</strong>
                        </p>
                        {criteriaSetInfo && (
                            <p className="text-sm text-gray-500 mt-1">
                                Criterios de: <strong>{criteriaSetInfo.subject} - {criteriaSetInfo.course}</strong>
                            </p>
                        )}
                    </div>
                    <div className="flex items-center space-x-4 flex-shrink-0">
                         <button onClick={() => openEditRubricModal(currentRubric)} className="bg-orange-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-orange-600">
                            Editar Rúbrica
                        </button>
                        <button onClick={generateReportPreview} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">
                            Ver Informe
                        </button>
                        <button onClick={() => setCurrentPage('main')} className="bg-gray-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-600">Volver</button>
                    </div>
                </header>
                <div className="bg-white p-6 rounded-2xl shadow-lg">
                    <input type="text" placeholder="Buscar estudiante..." value={searchTerm} onChange={e => setSearchTerm(e.target.value)} className="w-full max-w-sm p-2 border rounded-lg mb-4" />
                    <div className="max-h-[60vh] overflow-y-auto space-y-2">
                        {(currentClass?.students || [])
                            .filter(s => s.name.toLowerCase().includes(searchTerm.toLowerCase()))
                            .map(student => {
                                const score = calculateStudentScore(student.id);
                                const scoreText = score === null ? 'Pendiente' : `${score.toFixed(2)} / 10`;
                                const scoreClasses = score === null ? 'bg-gray-200 text-gray-600' : score < 5 ? 'bg-red-600 text-white' : 'bg-blue-100 text-blue-800';
                                return (
                                    <div key={student.id} className="flex justify-between items-center p-3 bg-gray-50 rounded-lg hover:bg-gray-100">
                                        <span>{student.name}</span>
                                        <div className="flex items-center space-x-3">
                                            <span className={`font-bold text-sm px-3 py-1 rounded-full ${scoreClasses}`}>
                                                {scoreText}
                                            </span>
                                            <button onClick={() => openEvaluationModal(student)} className="bg-teal-500 text-white font-bold py-1 px-3 rounded-lg hover:bg-teal-600">Evaluar</button>
                                        </div>
                                    </div>
                                );
                        })}
                    </div>
                </div>
            </div>
        )
    };

    const renderReportPreviewPage = () => {
        if (!reportPreviewData) return null;
        const { rubric, class: classData, evaluations: allEvaluations, calculateStudentScore: scoreCalculator, activityTitle, selectedCriteria } = reportPreviewData;

        return (
            <div className="p-4 sm:p-8">
                <header className="flex justify-between items-center mb-6">
                    <h1 className="text-3xl font-bold text-gray-800">Evaluación de la actividad: {activityTitle}</h1>
                    <div className="flex space-x-4">
                         <button onClick={() => setCurrentPage('evaluation')} className="bg-gray-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-600">
                            Volver a Evaluación
                        </button>
                        <button onClick={exportPdf} disabled={!isJsPDFLoaded} className="bg-green-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-green-700 disabled:bg-gray-400">
                            {isJsPDFLoaded ? 'Exportar a PDF' : 'Cargando...'}
                        </button>
                    </div>
                </header>

                <div className="bg-white p-6 rounded-2xl shadow-lg space-y-12">
                    {/* Evaluation Criteria */}
                    {selectedCriteria && selectedCriteria.length > 0 && (
                        <div>
                            <h3 className="text-xl font-semibold mb-4">Criterios de Evaluación Aplicados</h3>
                            <div className="overflow-x-auto">
                                <table className="min-w-full text-sm border">
                                    <thead className="bg-gray-100">
                                        <tr>
                                            <th className="border p-2 text-left font-bold w-1/4">Código</th>
                                            <th className="border p-2 text-left font-bold">Descripción</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        {selectedCriteria.map((c, i) => (
                                            <tr key={i}>
                                                <td className="border p-2 align-top">{c.code}</td>
                                                <td className="border p-2 align-top">{c.text}</td>
                                            </tr>
                                        ))}
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    )}

                    {/* Full Rubric */}
                    <div>
                        <h3 className="text-xl font-semibold mb-4">Rúbrica Completa: {rubric.name}</h3>
                        <div className="overflow-x-auto">
                            <table className="min-w-full text-sm border">
                                <thead className="bg-gray-100">
                                    <tr>
                                        <th className="border p-2 text-left font-bold">Indicador</th>
                                        {rubric.levels.map((level, i) => <th key={i} className="border p-2 text-left font-bold">{parseMarkdown(level, false)}</th>)}
                                    </tr>
                                </thead>
                                <tbody>
                                    {rubric.criteria.map(c => (
                                        <tr key={c.id}>
                                            <td className="border p-2 font-bold align-top">{parseMarkdown(c.name)}</td>
                                            {c.levels.map((desc, i) => <td key={i} className="border p-2 align-top">{desc}</td>)}
                                        </tr>
                                    ))}
                                </tbody>
                            </table>
                        </div>
                    </div>

                    {/* General Scores */}
                    <div>
                        <h3 className="text-xl font-semibold mb-4">Puntuaciones Generales: {classData.name}</h3>
                        <div className="overflow-x-auto">
                            <table className="min-w-full bg-white border">
                                <thead className="bg-gray-200">
                                    <tr>
                                        <th className="py-2 px-4 border">Estudiante</th>
                                        <th className="py-2 px-4 border">Puntuación (/10)</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    {classData.students.map(student => {
                                        const score = scoreCalculator(student.id);
                                        const scoreDisplay = score === null ? 'Pendiente' : score.toFixed(2);
                                        const isFail = score !== null && score < 5;
                                        return (
                                            <tr key={student.id} className="text-center">
                                                <td className="py-2 px-4 border">{student.name}</td>
                                                <td className={`py-2 px-4 border font-mono ${isFail ? 'bg-red-600 text-white' : ''}`}>{scoreDisplay}</td>
                                            </tr>
                                        )
                                    })}
                                </tbody>
                            </table>
                        </div>
                    </div>
                    
                    {/* Individual Reports */}
                    <div>
                         <h3 className="text-xl font-semibold mb-4">Informes Individuales</h3>
                         <div className="space-y-8">
                            {classData.students.map(student => (
                                <div key={student.id}>
                                    <h4 className="font-bold">{student.name}</h4>
                                    <table className="min-w-full text-sm border mt-2">
                                        <thead className="bg-gray-100">
                                            <tr>
                                                <th className="border p-2 text-left font-bold">Indicador</th>
                                                {rubric.levels.map((level, i) => <th key={i} className="border p-2 text-left font-bold">{parseMarkdown(level, false)}</th>)}
                                            </tr>
                                        </thead>
                                        <tbody>
                                            {rubric.criteria.map(c => (
                                                <tr key={c.id}>
                                                    <td className="border p-2 font-bold">{parseMarkdown(c.name, false)}</td>
                                                    {rubric.levels.map((l, levelIdx) => (
                                                         <td key={levelIdx} className={`border p-4 ${allEvaluations[student.id]?.[c.id] === levelIdx ? 'bg-orange-200' : ''}`}></td>
                                                    ))}
                                                </tr>
                                            ))}
                                        </tbody>
                                    </table>
                                </div>
                            ))}
                         </div>
                    </div>
                </div>
            </div>
        );
    };
    
    const renderEvaluationModal = () => {
        if (!showEvalModal || !selectedStudent || !currentRubric) return null;
        
        const score = calculateStudentScore(selectedStudent.id, tempEvaluation);
        const currentIndex = currentClass.students.findIndex(s => s.id === selectedStudent.id);
        const isLastStudent = currentIndex === currentClass.students.length - 1;

        return (
            <Modal onClose={handleRequestCloseEvalModal} size="max-w-6xl">
                 <div className="p-6 overflow-y-auto" key={selectedStudent.id}>
                    <button onClick={handleRequestCloseEvalModal} className="absolute top-4 right-4 text-gray-400 hover:text-gray-600 z-10">
                        <svg className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                    <h2 className="text-2xl font-bold mb-4">{selectedStudent.name}</h2>
                    <div className="overflow-x-auto mb-4">
                        <table className="min-w-full border-collapse">
                            <thead>
                                <tr className="bg-gray-100">
                                    <th className="border p-2 text-left font-bold">Indicador</th>
                                    {currentRubric.levels.map((level, i) => <th key={i} className="border p-2 font-bold">{parseMarkdown(level, false)}</th>)}
                                </tr>
                            </thead>
                            <tbody>
                                {currentRubric.criteria.map(c => (
                                    <tr key={c.id}>
                                        <td className="border p-2 font-bold align-top">{parseMarkdown(c.name)}</td>
                                        {c.levels.map((desc, levelIdx) => (
                                            <td key={levelIdx} className={`border p-2 cursor-pointer hover:bg-blue-200 transition-colors ${tempEvaluation?.[c.id] === levelIdx ? 'bg-blue-300' : ''}`} onClick={() => handleCellClick(c.id, levelIdx)}>
                                                <p className="text-sm">{desc}</p>
                                            </td>
                                        ))}
                                    </tr>
                                ))}
                            </tbody>
                        </table>
                    </div>
                    <div className="bg-gray-50 p-4 rounded-lg">
                        <div className="flex justify-between items-center mb-2">
                             <h3 className="text-lg font-semibold">Feedback con IA</h3>
                             <button onClick={generateStudentFeedback} disabled={isGeneratingFeedback} className="flex items-center bg-green-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-green-700">
                                {isGeneratingFeedback ? <ButtonSpinner /> : 'Generar Feedback'}
                            </button>
                        </div>
                        {llmFeedback && <div className="whitespace-pre-wrap p-3 bg-white rounded border">{parseMarkdown(llmFeedback, false)}</div>}
                    </div>
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-between items-center">
                     {score !== null && (
                         <div className={`font-bold text-lg p-3 rounded-lg shadow-lg ${score < 5 ? 'bg-red-600' : 'bg-orange-500'} text-white`}>
                            Puntuación: {score.toFixed(2)}
                        </div>
                    )}
                    <div className="flex justify-end space-x-3 flex-grow">
                        <button onClick={handleSaveEvaluation} className="bg-blue-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-600">Guardar y Cerrar</button>
                        <button onClick={handleSaveAndNext} disabled={isLastStudent} className="bg-blue-700 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-800 disabled:bg-gray-400">Guardar y Siguiente</button>
                    </div>
                </div>
            </Modal>
        );
    };

    const renderCreateClassModal = () => {
        if (!showCreateClassModal) return null;
        return (
            <Modal onClose={() => setShowCreateClassModal(false)} size="max-w-lg">
                <div className="p-6 space-y-4">
                    <h2 className="text-xl font-bold">Crear Nueva Clase</h2>
                    <input type="text" value={classNameInput} onChange={e => setClassNameInput(e.target.value)} placeholder="Nombre de la clase" className="w-full p-2 border rounded-lg" />
                    <textarea value={studentsInput} onChange={e => setStudentsInput(e.target.value)} placeholder="Un nombre de estudiante por línea..." className="w-full p-2 border rounded-lg h-40" />
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-end space-x-2">
                    <button onClick={() => setShowCreateClassModal(false)} className="bg-gray-500 text-white py-2 px-4 rounded-lg">Cancelar</button>
                    <button onClick={handleSaveClass} className="bg-blue-600 text-white py-2 px-4 rounded-lg">Guardar Clase</button>
                </div>
            </Modal>
        );
    };

    const renderEditClassModal = () => {
        if (!showEditClassModal || !classToEdit) return null;
        return (
            <Modal onClose={() => setShowEditClassModal(false)} size="max-w-lg">
                <div className="p-6">
                    <h2 className="text-xl font-bold mb-4">Editar Clase: {classToEdit.name}</h2>
                    <div className="space-y-2 max-h-60 overflow-y-auto mb-4 pr-2">
                        {classToEdit.students.map(student => (
                            <div key={student.id} className="flex justify-between items-center bg-gray-100 p-2 rounded">
                                <span>{student.name}</span>
                                <button onClick={() => removeStudentFromEditList(student.id)} className="text-red-500 hover:text-red-700 font-bold">X</button>
                            </div>
                        ))}
                    </div>
                    <div className="flex space-x-2 mt-4">
                        <input type="text" value={newStudentName} onChange={e => setNewStudentName(e.target.value)} placeholder="Nuevo estudiante" className="flex-grow p-2 border rounded-lg" />
                        <button onClick={addStudentToEditList} className="bg-blue-600 text-white px-4 rounded-lg hover:bg-blue-700">Añadir</button>
                    </div>
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-end space-x-2">
                    <button onClick={() => setShowEditClassModal(false)} className="bg-gray-500 text-white py-2 px-4 rounded-lg">Cancelar</button>
                    <button onClick={handleUpdateClass} className="bg-blue-600 text-white py-2 px-4 rounded-lg">Guardar Cambios</button>
                </div>
            </Modal>
        );
    };

    const renderCreateEvalCriteriaModal = () => {
        if (!showCreateEvalCriteriaModal) return null;
        const isImportDisabled = !newCriteriaSubject.trim() || !newCriteriaCourse.trim();
        return (
            <Modal onClose={() => setShowCreateEvalCriteriaModal(false)} size="max-w-2xl">
                <div className="p-6 flex-grow overflow-y-auto">
                    <h2 className="text-xl font-bold mb-4">{criteriaToEdit ? 'Editar' : 'Crear'} Criterios de Evaluación</h2>
                    <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                        <input type="text" value={newCriteriaSubject} onChange={e => setNewCriteriaSubject(e.target.value)} placeholder="Asignatura (e.g., Matemáticas)" className="w-full p-2 border rounded-lg" />
                        <input type="text" value={newCriteriaCourse} onChange={e => setNewCriteriaCourse(e.target.value)} placeholder="Curso (e.g., 4º ESO)" className="w-full p-2 border rounded-lg" />
                    </div>
                    <div className="space-y-2">
                        {tableCriteria.map((row, index) => (
                            <div key={row.id} className="grid grid-cols-[1fr_3fr_auto] gap-2 items-center">
                                <input type="text" value={row.code} onChange={e => handleCriteriaRowChange(row.id, 'code', e.target.value)} placeholder={`Código ${index + 1}`} className="p-2 border rounded-lg"/>
                                <textarea value={row.text} onChange={e => handleCriteriaRowChange(row.id, 'text', e.target.value)} placeholder={`Descripción del criterio ${index + 1}`} className="p-2 border rounded-lg h-16 resize-none"/>
                                <button onClick={() => removeCriteriaRow(row.id)} className="text-red-500 hover:text-red-700 font-bold p-2">X</button>
                            </div>
                        ))}
                    </div>
                    <button onClick={addCriteriaRow} className="mt-4 text-blue-600 hover:underline">+ Añadir Criterio</button>
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-between items-center flex-shrink-0">
                    <button 
                        onClick={() => setShowImportCriteriaModal(true)} 
                        disabled={isImportDisabled}
                        className="bg-gray-600 text-white py-2 px-4 rounded-lg hover:bg-gray-700 disabled:bg-gray-400 disabled:cursor-not-allowed"
                        title={isImportDisabled ? "Introduce Asignatura y Curso para activar" : "Importar criterios desde un archivo Markdown"}
                    >
                        Importar desde MD
                    </button>
                    <div className="space-x-2">
                        <button onClick={() => setShowCreateEvalCriteriaModal(false)} className="bg-gray-500 text-white py-2 px-4 rounded-lg">Cancelar</button>
                        <button onClick={handleSaveEvalCriteria} className="bg-purple-600 text-white py-2 px-4 rounded-lg">Guardar Criterios</button>
                    </div>
                </div>
            </Modal>
        );
    }

    const renderImportCriteriaModal = () => {
        if (!showImportCriteriaModal) return null;
    
        return (
            <Modal onClose={() => setShowImportCriteriaModal(false)} size="max-w-lg" customClasses="z-50">
                <div className="p-6">
                    <h2 className="text-xl font-bold mb-4">Importar Criterios desde Markdown</h2>
                    <p className="text-gray-600 mb-4">
                        Selecciona un archivo .md que contenga una tabla. La primera columna será el 'Código' y la segunda la 'Descripción'. La cabecera de la tabla será ignorada.
                    </p>
                    <label className="w-full flex flex-col items-center px-4 py-6 bg-white text-blue-500 rounded-lg shadow-lg tracking-wide uppercase border border-blue-500 cursor-pointer hover:bg-blue-500 hover:text-white">
                        <svg className="w-8 h-8" fill="currentColor" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                            <path d="M16.88 9.1A4 4 0 0 1 16 17H5a5 5 0 0 1-1-9.9V7a3 3 0 0 1 4.52-2.59A4.98 4.98 0 0 1 17 8c0 .38-.04.74-.12 1.1zM11 11h3l-4 4-4-4h3v-3h2v3z" />
                        </svg>
                        <span className="mt-2 text-base leading-normal">{criteriaMdFile ? criteriaMdFile.name : "Seleccionar archivo"}</span>
                        <input type='file' className="hidden" accept=".md" onChange={e => setCriteriaMdFile(e.target.files[0])} />
                    </label>
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-end space-x-3">
                    <button onClick={() => setShowImportCriteriaModal(false)} className="bg-gray-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-600">Cancelar</button>
                    <button onClick={handleProcessAndSaveImportedCriteria} disabled={!criteriaMdFile} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700 disabled:bg-gray-400">
                        Procesar y Guardar
                    </button>
                </div>
            </Modal>
        );
    };

    const renderEditRubricModal = () => {
        if (!showEditRubricModal || !editableRubricData) return null;
        
        const renderCriterionRow = (criterion, critIdx) => {
            const { cleanName, weight } = criterion;
            return (
                <tr key={criterion.id}>
                    <td className="border p-1 align-top">
                        <textarea 
                            value={cleanName} 
                            onChange={e => handleRubricDataChange(critIdx, 'name', e.target.value)} 
                            className="w-full p-1 h-24 bg-green-50 rounded-md font-bold"
                        />
                        <input 
                            type="number"
                            value={weight === null ? '' : weight}
                            onChange={e => handleRubricDataChange(critIdx, 'weight', e.target.value)}
                            placeholder="%"
                            className="w-full p-1 mt-1 text-center rounded-md border bg-green-600 text-white placeholder-white [appearance:textfield] [&::-webkit-outer-spin-button]:appearance-none [&::-webkit-inner-spin-button]:appearance-none"
                        />
                    </td>
                    {criterion.levels.map((desc, levelIdx) => (
                        <td key={levelIdx} className="border p-1 align-top">
                            <textarea 
                                value={desc} 
                                onChange={e => {
                                    const newCriteria = [...editableRubricData.criteria];
                                    newCriteria[critIdx].levels[levelIdx] = e.target.value;
                                    setEditableRubricData(prev => ({ ...prev, criteria: newCriteria }));
                                }} 
                                className="w-full p-1 h-full min-h-[120px] rounded-md"
                            />
                        </td>
                    ))}
                </tr>
            );
        };

        return (
            <Modal onClose={() => setShowEditRubricModal(false)} size="max-w-7xl">
                <div className="p-6 overflow-auto">
                    <input 
                        type="text" 
                        value={editableRubricData.name} 
                        onChange={e => {
                            setEditableRubricData(prev => ({...prev, name: e.target.value}));
                        }}
                        className="w-full p-2 text-2xl font-bold mb-4 border-b-2"
                        placeholder="Título de la Rúbrica"
                    />
                    <div className="overflow-x-auto">
                        <table className="min-w-full border-collapse">
                            <thead>
                                <tr>
                                    <th className="border p-1">Indicador</th>
                                    {editableRubricData.levels.map((level, i) => (
                                        <th key={i} className="border p-1">
                                            <input 
                                                type="text" 
                                                value={level.replace(/\*\*/g, '')} 
                                                onChange={e => {
                                                    const newLevels = [...editableRubricData.levels];
                                                    newLevels[i] = `**${e.target.value}**`;
                                                    setEditableRubricData(prev => ({...prev, levels: newLevels}));
                                                }} 
                                                className="w-full p-1 font-bold bg-yellow-100 rounded-md"
                                            />
                                        </th>
                                    ))}
                                </tr>
                            </thead>
                            <tbody>
                                {editableRubricData.criteria.map(renderCriterionRow)}
                            </tbody>
                        </table>
                    </div>
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-end space-x-2">
                    <button onClick={() => setShowEditRubricModal(false)} className="bg-gray-500 text-white py-2 px-4 rounded-lg">Cancelar</button>
                    <button onClick={handleUpdateRubric} className="bg-blue-600 text-white py-2 px-4 rounded-lg">Guardar Cambios</button>
                </div>
            </Modal>
        );
    };

    const renderActivityTitleModal = () => {
        if (!showActivityTitleModal) return null;

        const handleToggleCriterion = (criterion) => {
            setSelectedCriteriaForActivity(prev => {
                const isSelected = prev.some(c => c.code === criterion.code);
                if (isSelected) {
                    return prev.filter(c => c.code !== criterion.code);
                } else {
                    return [...prev, criterion];
                }
            });
        };
        
        const availableCriteria = savedEvalCriteria.find(s => s.id === criteriaSetToAssign)?.criteria || [];

        return (
            <Modal onClose={() => setShowActivityTitleModal(false)} size="max-w-2xl">
                <div className="p-6 space-y-4 flex-grow overflow-y-auto">
                    <h2 className="text-xl font-bold">Crear Actividad de Evaluación</h2>
                    <p className="text-gray-600">Introduce un título y selecciona los criterios de evaluación para esta sesión.</p>
                    <div>
                        <label className="font-semibold text-sm">Título de la Actividad</label>
                        <input 
                            type="text" 
                            value={activityTitleInput} 
                            onChange={e => setActivityTitleInput(e.target.value)} 
                            placeholder="Ej: Ensayo sobre la Edad Media" 
                            className="w-full p-2 border rounded-lg mt-1"
                        />
                    </div>
                     <div>
                        <label className="font-semibold text-sm">Conjunto de Criterios de Evaluación (Opcional)</label>
                        <select 
                            value={criteriaSetToAssign} 
                            onChange={e => {
                                setCriteriaSetToAssign(e.target.value);
                                setSelectedCriteriaForActivity([]); // Reset selection when set changes
                            }} 
                            className="w-full p-2 border rounded-lg mt-1"
                        >
                            <option value="">Ninguno</option>
                            {savedEvalCriteria.map(c => (
                                <option key={c.id} value={c.id}>{c.subject} - {c.course}</option>
                            ))}
                        </select>
                    </div>

                    {availableCriteria.length > 0 && (
                        <div className="border-t pt-4 mt-4">
                            <div className='flex justify-between items-center mb-2'>
                                <h3 className="font-semibold text-sm">Selecciona los criterios a aplicar:</h3>
                                <button onClick={handleSuggestCriteria} disabled={isSuggestingCriteria || !activityTitleInput || !criteriaSetToAssign} className="bg-indigo-600 text-white font-bold py-1 px-3 rounded-lg hover:bg-indigo-700 disabled:bg-gray-400 flex items-center">
                                    {isSuggestingCriteria ? <ButtonSpinner /> : 'Sugerir Criterios'}
                                </button>
                            </div>
                            <div className="max-h-60 overflow-y-auto space-y-2 pr-2">
                                {availableCriteria.map((crit, index) => (
                                    <label key={index} className="flex items-center p-2 rounded-lg hover:bg-gray-100 cursor-pointer">
                                        <input 
                                            type="checkbox"
                                            className="h-5 w-5 rounded border-gray-300 text-blue-600 focus:ring-blue-500"
                                            checked={selectedCriteriaForActivity.some(c => c.code === crit.code)}
                                            onChange={() => handleToggleCriterion(crit)}
                                        />
                                        <span className="ml-3 text-sm">
                                            <strong className="font-bold">{crit.code}:</strong> {crit.text}
                                        </span>
                                    </label>
                                ))}
                            </div>
                        </div>
                    )}
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-end space-x-3 flex-shrink-0">
                    <button onClick={() => setShowActivityTitleModal(false)} className="bg-gray-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-600">Cancelar</button>
                    <button onClick={handleStartEvaluation} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Continuar</button>
                </div>
            </Modal>
        );
    };

    const renderInfoModal = () => {
        if (!showInfoModal) return null;
        return (
            <Modal onClose={() => setShowInfoModal(false)} size="max-w-lg">
                <div className="p-6">
                    <h2 className="text-xl font-bold mb-4">Información</h2>
                    <p className="text-gray-700">
                        Para importar una rúbrica, esta ha de estar en formato .md, para ello, puedes hacerlo directamente desde un documento de Google y descargar como Markdown (.md). 
                        En este <a href="https://docs.google.com/document/d/1FFk-SryXzlgy3oQPxT8PqOaPgqJ73-uZc4RePg_BXYQ/edit?usp=sharing" target="_blank" rel="noopener noreferrer" className="text-blue-600 hover:underline font-semibold">enlace</a> encontrarás un ejemplo.
                    </p>
                </div>
                 <div className="p-4 bg-gray-50 border-t flex justify-end">
                    <button onClick={() => setShowInfoModal(false)} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Entendido</button>
                </div>
            </Modal>
        );
    };

    const renderDuplicateRubricModal = () => {
        if (!showDuplicateRubricModal) return null;
        return (
            <Modal onClose={() => setShowDuplicateRubricModal(false)} size="max-w-lg">
                <div className="p-6">
                    <h2 className="text-xl font-bold mb-4">Nombre de Rúbrica Duplicado</h2>
                    <p className="mb-4 text-gray-600">Ya existe una rúbrica llamada "{pendingRubric?.name}". Por favor, introduce un nuevo nombre para guardar esta rúbrica.</p>
                    <input 
                        type="text" 
                        value={newRubricName} 
                        onChange={e => setNewRubricName(e.target.value)} 
                        className="w-full p-2 border rounded-lg"
                    />
                </div>
                <div className="p-4 bg-gray-50 border-t flex justify-end space-x-3">
                    <button onClick={() => setShowDuplicateRubricModal(false)} className="bg-gray-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-gray-600">Cancelar</button>
                    <button onClick={handleSaveWithNewName} className="bg-blue-600 text-white font-bold py-2 px-4 rounded-lg hover:bg-blue-700">Guardar con nuevo nombre</button>
                </div>
            </Modal>
        );
    };
    
    const renderPage = () => {
        switch (currentPage) {
            case 'evaluation':
                return renderEvaluationPage();
            case 'reportPreview':
                return renderReportPreviewPage();
            case 'main':
            default:
                return renderMainPage();
        }
    };

    return (
        <div className="bg-gray-50 min-h-screen font-sans">
            <Message text={message.text} type={message.type} />
            {showDeleteConfirmModal && itemToDelete && (
                <ConfirmationModal 
                    title={itemToDelete.type === 'class' ? 'Confirmar borrado de clase' : `Confirmar Borrado de ${itemToDelete.type}`}
                    message={`¿Estás seguro de que quieres eliminar "${itemToDelete.name}"? Esta acción no se puede deshacer.`}
                    onConfirm={handleConfirmDelete}
                    onCancel={() => setShowDeleteConfirmModal(false)}
                />
            )}
            {showDependencyModal && <DependencyModal name={dependencyInfo.name} activities={dependencyInfo.activities} onClose={() => setShowDependencyModal(false)} />}
            {showValidationErrorModal && <InfoModal title={validationErrorInfo.title} message={validationErrorInfo.message} onClose={() => setShowValidationErrorModal(false)} customClasses="z-50" />}
            {showCloseEvalConfirmModal && (
                <ConfirmationModal
                    title="¿Cerrar Evaluación?"
                    message="¿Estás seguro que quieres cerrar la rúbrica? Se perderán las modificaciones."
                    onConfirm={() => {
                        setShowEvalModal(false);
                        setShowCloseEvalConfirmModal(false);
                    }}
                    onCancel={() => setShowCloseEvalConfirmModal(false)}
                    confirmText="Aceptar"
                    cancelText="Cancelar"
                />
            )}
            {renderPage()}
            {showEvalModal && renderEvaluationModal()}
            {showCreateClassModal && renderCreateClassModal()}
            {showEditClassModal && renderEditClassModal()}
            {showCreateEvalCriteriaModal && renderCreateEvalCriteriaModal()}
            {showImportCriteriaModal && renderImportCriteriaModal()}
            {showEditRubricModal && renderEditRubricModal()}
            {showActivityTitleModal && renderActivityTitleModal()}
            {showInfoModal && renderInfoModal()}
            {showDuplicateRubricModal && renderDuplicateRubricModal()}
        </div>
    );
};

export default App;
