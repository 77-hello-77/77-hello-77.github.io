# 77-hello-77.github.io
You can add names and dates. 
import { useState } from 'react';
import { Button } from './components/ui/button';
import { Input } from './components/ui/input';
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from './components/ui/table';
import { Trash2 } from 'lucide-react';

interface Entry {
  id: number;
  name: string;
  date: string;
}

export default function App() {
  const [entries, setEntries] = useState<Entry[]>([]);
  const [name, setName] = useState('');
  const [date, setDate] = useState('');

  const addEntry = () => {
    if (name.trim() && date.trim()) {
      const newEntry: Entry = {
        id: Date.now(),
        name: name.trim(),
        date: date.trim(),
      };
      setEntries([...entries, newEntry]);
      setName('');
      setDate('');
    }
  };

  const deleteEntry = (id: number) => {
    setEntries(entries.filter(entry => entry.id !== id));
  };

  const handleKeyPress = (e: React.KeyboardEvent) => {
    if (e.key === 'Enter') {
      addEntry();
    }
  };

  return (
    <div className="size-full flex items-center justify-center bg-gray-50">
      <div className="w-full max-w-3xl p-8">
        <div className="bg-white rounded-lg shadow-lg p-6">
          <h1 className="text-3xl mb-6">Name and Date Entry</h1>
          
          <div className="flex gap-4 mb-6">
            <div className="flex-1">
              <label htmlFor="name" className="block text-sm mb-2">
                Name
              </label>
              <Input
                id="name"
                type="text"
                placeholder="Enter name"
                value={name}
                onChange={(e) => setName(e.target.value)}
                onKeyPress={handleKeyPress}
              />
            </div>
            <div className="flex-1">
              <label htmlFor="date" className="block text-sm mb-2">
                Date
              </label>
              <Input
                id="date"
                type="date"
                value={date}
                onChange={(e) => setDate(e.target.value)}
                onKeyPress={handleKeyPress}
              />
            </div>
            <div className="flex items-end">
              <Button onClick={addEntry}>
                Add Entry
              </Button>
            </div>
          </div>

          {entries.length > 0 && (
            <div className="border rounded-lg">
              <Table>
                <TableHeader>
                  <TableRow>
                    <TableHead>Name</TableHead>
                    <TableHead>Date</TableHead>
                    <TableHead className="w-[100px]">Actions</TableHead>
                  </TableRow>
                </TableHeader>
                <TableBody>
                  {entries.map((entry) => (
                    <TableRow key={entry.id}>
                      <TableCell>{entry.name}</TableCell>
                      <TableCell>{entry.date}</TableCell>
                      <TableCell>
                        <Button
                          variant="ghost"
                          size="sm"
                          onClick={() => deleteEntry(entry.id)}
                        >
                          <Trash2 className="h-4 w-4 text-red-500" />
                        </Button>
                      </TableCell>
                    </TableRow>
                  ))}
                </TableBody>
              </Table>
            </div>
          )}

          {entries.length === 0 && (
            <div className="text-center text-gray-500 py-8">
              No entries yet. Add your first entry above!
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

