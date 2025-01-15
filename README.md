import React, { useState, useEffect } from 'react';
import { Save, Trash, Bold, Italic, List, Heading, Quote, Code } from 'lucide-react';

const Notepad = () => {
  const [notes, setNotes] = useState([]);
  const [currentNote, setCurrentNote] = useState('');

  useEffect(() => {
    const savedNotes = localStorage.getItem('notes');
    if (savedNotes) {
      setNotes(JSON.parse(savedNotes));
    }
  }, []);

  useEffect(() => {
    localStorage.setItem('notes', JSON.stringify(notes));
  }, [notes]);

  const handleSaveNote = () => {
    if (currentNote.trim()) {
      setNotes([...notes, {
        id: Date.now(),
        content: currentNote,
        date: new Date().toLocaleString()
      }]);
      setCurrentNote('');
    }
  };

  const handleDeleteNote = (id) => {
    setNotes(notes.filter(note => note.id !== id));
  };

  const formatText = (command, value = null) => {
    document.execCommand(command, false, value);
  };

  return (
    <div className="max-w-4xl mx-auto p-4 min-h-screen bg-sky-200">
      <div className="mb-8">
        <h1 className="text-3xl font-bold mb-4 text-gray-800">Simple Notepad</h1>
        
        {/* Text Formatting Toolbar */}
        <div className="bg-gray-100 p-2 rounded-t-lg border border-b-0 flex gap-2 flex-wrap">
          <button
            onClick={() => formatText('bold')}
            className="p-2 hover:bg-white rounded transition-colors"
            title="Bold"
          >
            <Bold size={20} />
          </button>
          <button
            onClick={() => formatText('italic')}
            className="p-2 hover:bg-white rounded transition-colors"
            title="Italic"
          >
            <Italic size={20} />
          </button>
          <button
            onClick={() => formatText('insertUnorderedList')}
            className="p-2 hover:bg-white rounded transition-colors"
            title="Bullet List"
          >
            <List size={20} />
          </button>
          <button
            onClick={() => formatText('formatBlock', '<h2>')}
            className="p-2 hover:bg-white rounded transition-colors"
            title="Heading"
          >
            <Heading size={20} />
          </button>
          <button
            onClick={() => formatText('formatBlock', '<blockquote>')}
            className="p-2 hover:bg-white rounded transition-colors"
            title="Quote"
          >
            <Quote size={20} />
          </button>
          <button
            onClick={() => formatText('formatBlock', '<pre>')}
            className="p-2 hover:bg-white rounded transition-colors"
            title="Code Block"
          >
            <Code size={20} />
          </button>
        </div>

        {/* Note Input Area */}
        <div className="mb-4">
          <div
            contentEditable="true"
            onInput={(e) => setCurrentNote(e.currentTarget.innerHTML)}
            className="w-full min-h-32 p-3 border rounded-b-lg shadow-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500 bg-white"
            style={{ outline: 'none' }}
          />
          <div className="mt-2 flex justify-end">
            <button
              onClick={handleSaveNote}
              className="flex items-center gap-2 bg-blue-500 text-white px-4 py-2 rounded-lg hover:bg-blue-600 transition-colors"
            >
              <Save size={20} />
              Save Note
            </button>
          </div>
        </div>

        {/* Notes List */}
        <div className="space-y-4">
          {notes.map(note => (
            <div key={note.id} className="bg-white p-4 rounded-lg shadow">
              <div className="flex justify-between items-start">
                <div 
                  className="whitespace-pre-wrap text-gray-700"
                  dangerouslySetInnerHTML={{ __html: note.content }}
                />
                <button
                  onClick={() => handleDeleteNote(note.id)}
                  className="text-red-500 hover:text-red-600 p-1"
                >
                  <Trash size={20} />
                </button>
              </div>
              <p className="text-sm text-gray-500 mt-2">{note.date}</p>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default Notepad;
