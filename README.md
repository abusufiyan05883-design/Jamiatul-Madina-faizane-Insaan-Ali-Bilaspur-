# Jamiatul-Madina-faizane-Insaan-Ali-Bilaspur-
Jamiatul Madina Faizane Insaan Ali Bilaspur Official Educational &amp; Management System
import { useState, useEffect } from 'react';
import { Input } from '@/components/ui/input';
import { Button } from '@/components/ui/button';
import { Label } from '@/components/ui/label';
import { AppData, InstituteSettings } from '@/lib/store';
import { Save, Phone } from 'lucide-react';
import { toast } from 'sonner';

interface Props {
  data: AppData;
  update: (fn: (d: AppData) => AppData) => void;
}

export function InstituteSettingsPanel({ data, update }: Props) {
  const [form, setForm] = useState<InstituteSettings>(data.institute);

  useEffect(() => { setForm(data.institute); }, [data.institute]);

  const set = (k: keyof InstituteSettings, v: string) => setForm((f) => ({ ...f, [k]: v }));

  const save = () => {
    update((d) => ({ ...d, institute: form }));
    toast.success('Institute details saved');
  };

  return (
    <div className="space-y-6 max-w-2xl">
      <div>
        <h2 className="text-xl font-bold flex items-center gap-2">
          <Phone className="h-5 w-5" /> Institute Contact & Details
        </h2>
        <p className="text-sm text-muted-foreground">
          These show on the header, class results and PDF report cards.
        </p>
      </div>

      <div className="space-y-4 bg-card border rounded-lg p-5">
        <div className="space-y-2">
          <Label htmlFor="fancy">Helpline / Fancy Number</Label>
          <Input id="fancy" value={form.fancyNumber} onChange={(e) => set('fancyNumber', e.target.value)} placeholder="e.g. 1800-123-4567" />
          <p className="text-xs text-muted-foreground">Toll-free / special display number</p>
        </div>

        <div className="space-y-2">
          <Label htmlFor="phone">Phone Number</Label>
          <Input id="phone" value={form.phone} onChange={(e) => set('phone', e.target.value)} placeholder="e.g. +91 99999 88888" />
        </div>

        <div className="space-y-2">
          <Label htmlFor="email">Email</Label>
          <Input id="email" type="email" value={form.email} onChange={(e) => set('email', e.target.value)} placeholder="info@jamiatulmadina.in" />
        </div>

        <div className="space-y-2">
          <Label htmlFor="address">Address</Label>
          <Input id="address" value={form.address} onChange={(e) => set('address', e.target.value)} placeholder="Bilaspur" />
        </div>

        <Button onClick={save} className="w-full md:w-auto">
          <Save className="h-4 w-4 mr-1" /> Save Details
        </Button>
      </div>
    </div>
  );
}
